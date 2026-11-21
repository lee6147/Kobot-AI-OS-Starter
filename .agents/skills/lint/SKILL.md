---
name: lint
description: "`Archive/` LLM Wiki 전체를 스캔해 깨진 링크·고아 페이지·중복·미착륙 파일·Frontmatter 5규약 표류·규모 상한(15)을 10가지 기준으로 점검하고, 고칠 목록을 심각도순으로 제안합니다. 사용자가 \"/lint\", \"위키 점검\", \"위키 정리\", \"건강 체크\", \"링크 깨진 거 확인\", \"index 맞는지 확인\" 등을 요청할 때 실행합니다. 자동 수정·자동 삭제 없이 진단 리포트를 먼저 제시하고, 사용자가 번호로 승인한 항목만 수정한 뒤 `Archive/wiki/index.md`와 `Archive/log.md`를 갱신합니다."
---

# lint — AI 백과사전(`Archive/`) 정기 건강 점검

> **파이프라인 위치:** `Archive/` 책장을 늘리지 않고 정돈하는 단계입니다 (`Clip 6.5.3` 근거).
> **핵심 원칙:** **진단은 AI가, 처방 승인은 사람이(Editor-in-Chief).** `/lint`의 1차 산출물은 고쳐진 볼트가 아니라 **사용자가 고를 진단 목록**입니다.

## ⚙️ 경로 설정 (`Kobot-AI-OS-Starter`)

| 이름 | 설정값 | 역할 |
|---|---|---|
| `WIKI_DIR` | `Archive/wiki/` | 검사 대상 위키 페이지 (`0_Me/_FRONTMATTER_REGS.md`에 등록된 내 카테고리 하위 폴더들) |
| `WIKI_INDEX` | `Archive/wiki/index.md` | **L1 카탈로그** — 등재·통계·마크 정합성 기준 |
| `RAW_DIR` | `Archive/raw/` | **불변 원본** — 내용 수정 절대 금지, 위치·인바운드 연결만 검사 |
| `RAW_SUBS` | `강의노트` · `데이터시트-논문` · `아티클` · `도서` · `assets` | `raw/` 허용 5분류 (이 밖의 `raw/` 직속 파일은 미착륙) |
| `WIKI_LOG` | `Archive/log.md` | 작업 원장 (`lint` 실행 시 항상 기록) |
| `ROOM_ROOT` | `Archive/` | 방 루트 (`AGENTS.md`, `log.md` 외 `.md`가 있으면 미착륙) |
| `OUTPUT_DIR` | `Outputs/` | 리포트를 파일로 남길 때의 단일 출구 (별도 `Archive/output/` 미사용) |
| `EXCLUDE` | `_TEMPLATES_/`, `_BOT_PACK_/`, `.agents/`, `Welcome/`, `AGENTS.md`, `GEMINI.md`, `CLAUDE.md`, `README.md` | 위키 카운트 및 고아 오탐 방지 제외 대상 |
| `PAGE_CAP` | `15` | 초기 위키 페이지 상한 (함정 `#4` 규모 함정 방어) |
| `STALE_DAYS` | `180` | 최종 수정일(`created`/`modified`) 기준 구버전(`stale`) 판정 일수 |
| `SYSTEM_FILES` | `index.md` · `AGENTS.md` | 시스템 파일 — 위키 페이지 수 및 고아 검사에서 제외 |

---

## 언제 사용하나요
- 주 1회 정기 건강 점검 (`AGENTS.md` 주간 루틴)
- `/ingest` 또는 `/brain-wiki-sync` 실행 전 위키 페이지 수가 상한(`PAGE_CAP = 15`)에 가까워졌을 때
- `/query`에서 있어야 할 문서를 못 찾거나 링크·통계가 어긋났을 때

---

## 에이전트 실행 절차

### 0. 스캔 준비 — 반드시 실물 목록 3개를 먼저 실측합니다
`Archive/wiki/index.md`에 적힌 숫자를 그대로 믿지 않고, 실제 디스크 파일 목록을 먼저 대조합니다:
1. **실제 위키 페이지 목록:** `Archive/wiki/<내 카테고리>/*.md` (`SYSTEM_FILES` 및 `.gitkeep` 제외)
2. **실제 `raw` 파일 목록:** `Archive/raw/` 직속 파일(미착륙 후보) 및 5개 하위 폴더 파일 (`.gitkeep` 제외)
3. **`index.md` 등재 항목:** `Archive/wiki/index.md` 내 `- [[Archive/wiki/...]]` 항목 수

**스캔 3원칙 (오탐 방지):**
1. **코드펜스(```) 및 인라인 백틱(`) 내부의 예시 `[[링크]]`는 깨진 링크 검사에서 제외**합니다.
2. **파일명 안의 `[대괄호]` 주의:** `2026-11-12 [출처] 제목`처럼 파일명에 대괄호가 들어 있을 수 있으므로 고정 문자열 매칭으로 실존 여부를 확인합니다.
3. **`SYSTEM_FILES`(`index.md`, `AGENTS.md`)는 페이지 카운트와 고아 검사에서 제외**하며, `index.md`에서 오는 링크는 고아 판정용 인바운드로 세지 않습니다.

---

### 1. 10가지 고정 검사 (`#1 ~ #10` 번호 고정)

| # | 검사 항목 | 점검 내용 | 심각도 |
|:--:|---|---|:--:|
| **1** | **깨진 wikilink** | `Archive/wiki/` 및 `index.md` 내 `[[링크]]`·`![[이미지]]` 대상 파일 실존 여부 | 🔴 Critical |
| **2** | **고아 페이지 (Orphan)** | `index.md` 미등재(`🔴`) 또는 `index.md` 외 인바운드/연결(`related`·`Ark`·`Lab`) 0건(`🟡`) | 🔴 / 🟡 |
| **3** | **Stale (구버전)** | `STALE_DAYS`(180일) 초과 방치 또는 `lint_flag: stale` 표시 문서 | 🟡 Warning |
| **4** | **중복 / 과잉 규칙 파일** | 제목·주제가 겹치는 위키 페이지, 동명 `raw` 파일(diff 대조 필수), 또는 하위 폴더의 중복 시스템 규칙(`AGENTS.md` 등) | 🟡 Warning |
| **5** | **빈 페이지 / 부실 명제** | 본문 100자 미만 또는 `## 핵심 요약` · `## 핵심 명제` 섹션 누락 | 🟡 Warning |
| **6** | **`index.md` 정합성** | 미등재 페이지, 유령 링크(파일 없음), `총 위키 페이지` 숫자 오기, 마크(`📥`·`💡`·`🔄`) 불일치 | 🔴 Critical |
| **7** | **Frontmatter 5규약** | `0_Me/_FRONTMATTER_REGS.md` 기준 `type: wiki`, `created`, `tags`(내 주제 태그), `status`(`draft|organized|shared`), `source` 누락·표류 | 🟡 Warning |
| **8** | **`raw` 미연결** | `Archive/raw/`(문서류) 중 어느 `wiki` 페이지도 `source`로 참조하지 않는 미컴파일 원본 | 🟡 Warning |
| **9** | **미착륙 파일** | `Archive/raw/` 루트 직속 파일(5분류 밖) 또는 `Archive/` 루트에 뜬 미분류 `.md` | 🟡 Warning |
| **10** | **규모 상한 & 흐름 정체** | 위키 페이지 `PAGE_CAP(15)` 근접/초과, `status: draft` 장기 적체 | 🟡 Warning |

---

### 2. 진단 리포트 출력 형식

```markdown
# 🧹 Archive Wiki Lint Report — YYYY-MM-DD

## 요약
| 항목 | 실측 | 권장 기준 | 판정 |
|---|:--:|:--:|:--:|
| 위키 페이지 (`WIKI_DIR`) | N개 | 5~15개 (`PAGE_CAP=15`) | 🟢 / 🟡 / 🔴 |
| 원본 파일 (`RAW_DIR`) | 문서 N개 (assets N개) | 미컴파일 0건 권장 | 🟢 / 🟡 |
| `index.md` 등재 정합 | N / N 일치 | 불일치 0건 | 🟢 / 🔴 |
| 깨진 링크 / 5규약 위반 | N건 / N건 | 0건 | 🟢 / 🔴 / 🟡 |

**종합:** 🔴 Critical N건 · 🟡 Warning N건 · 🟢 OK N항목

## 🔴 Critical (즉시 조치 필요)
- (없으면 "없음")

## 🟡 Warning (선택/정돈 권장)
- **[#번호 항목명]** 구체적 파일 경로와 원인 → **처방 제안 (A / B)**

## 🟢 OK (정상 통과 항목)
- 통과한 검사 번호 및 실측 근거 요약

## 추천 조치 (번호로 선택해 주세요)
1. [조치 후보 1]
2. [조치 후보 2 또는 "보고만 남기고 종료"]
```

---

### 3. 처방 실행 (사용자 번호 승인 후에만) & 원장 기록

1. **승인 우선:** 사용자가 번호로 승인한 항목만 수정합니다. (`Archive/raw/` 원본 내용은 절대 수정하지 않습니다.)
2. **`Archive/wiki/index.md` 갱신:** 점검 결과에 따라 `총 위키 페이지` 및 `마지막 갱신(또는 마지막 lint)` 정보를 최신화합니다.
3. **`Archive/log.md` 기록 (수정이 없어도 항상 append):**
   ```markdown
   ## [YYYY-MM-DD HH:MM] lint | 정기 건강 점검
   - **검사:** wiki N개 · raw 문서 N개 (assets N개) · 10가지 검사 완료
   - **발견:** 🔴 Critical N건 / 🟡 Warning N건 / 🟢 OK N항목
   - **조치:** [승인받아 수정한 내역 또는 "진단 보고만 수행"]
   - **미조치 이월:** [다음 점검으로 넘긴 항목]
   ```

---

## 절대 하지 않는 것
- ❌ 사용자 승인 없이 위키 파일이나 구조를 임의로 자동 수정·삭제하기
- ❌ `Archive/raw/` 원본 문서의 내용을 수정하기 (불변 원칙)
- ❌ 파일을 열어보지 않고 추측으로 "중복" 또는 "오류"라고 단정하기
- ❌ `Archive/log.md` 기록을 누락하기
