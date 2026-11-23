---
name: ingest
description: 외부 자료(강의 노트, 데이터시트, 논문, 아티클, 도서)를 `Archive/raw/`에 불변 보존한 뒤 `Archive/wiki/` 백과사전 페이지로 컴파일하고 `index.md`와 `log.md`를 함께 갱신합니다. 사용자가 "/ingest", "wiki에 넣어줘", "이 자료 위키로 정리해줘", "새 자료 흡수"를 요청할 때 실행합니다.
---

# ingest — 외부 자료를 AI 백과사전(`Archive/wiki/`)에 흡수하기

> 파이프라인 위치: **외부 자료(`Archive/raw/`)를 AI 책장(`Archive/wiki/`)에 꽂는 단계**입니다.
> 내가 직접 쓴 생각 노트는 이 경로가 아니라 `Ark/` → `/brain-wiki-sync` 경로로 들어옵니다.

## ⚙️ 경로 설정 (`Kobot-AI-OS-Starter`)

| 이름 | 설정값 | 설명 |
|---|---|---|
| `RAW_DIR` | `Archive/raw/` | 불변 원본 (`강의노트/`, `데이터시트-논문/`, `아티클/`, `도서/`, `assets/`) |
| `WIKI_DIR` | `Archive/wiki/` | AI 컴파일 위키 (`0_Me/_FRONTMATTER_REGS.md` 태그와 1:1 카테고리) |
| `WIKI_INDEX` | `Archive/wiki/index.md` | 위키 카탈로그 (소문자 `index.md` 주의) |
| `WIKI_LOG` | `Archive/log.md` | `Archive/` 루트의 append-only 작업 원장 |

---

## 에이전트 실행 절차

### 1. 입력 확인 및 `Archive/raw/` 원본 보존 (선행 필수)
입력 경로:
- `/ingest [Archive/raw/... 파일 경로]` → 기존 원본 재사용 (재복사하지 않음)
- `/ingest [Welcome/... 파일 경로]` → 외부 자료인지 확인한 뒤 `Archive/raw/<유형>/`으로 복사 후 처리 (Welcome 원본 유지)
- `/ingest [URL 또는 본문]` → `Archive/raw/<유형>/YYYY-MM-DD [출처] [제목].md`로 먼저 저장

**`Archive/raw/` 5개 유형 매핑:**
- 강의 노트·스터디 필기 → `Archive/raw/강의노트/`
- 데이터시트·논문·레퍼런스 매뉴얼 → `Archive/raw/데이터시트-논문/`
- 웹 클리핑·기술 아티클 → `Archive/raw/아티클/`
- 도서 원문·발췌 → `Archive/raw/도서/`
- 회로도·파형·다이어그램 이미지 → `Archive/raw/assets/`

> **절대 규칙:**
> - **외부 폴더 비파괴 원칙:** 볼트 외부 경로에서 자료를 가져올 때는 절대 이동·수정·삭제하지 않고 **반드시 `Archive/raw/`로 복사(Copy)**만 합니다.
> - **Just-In-Time 선별 복사:** 한 번에 수십 개를 복사하지 말고 당장 컴파일할 1~2건만 가져옵니다. (녹취 전문 제외, 핵심 요약과 대체 불가한 회로도·파형 이미지만 `assets/` 선별 복사)
> - **불변성:** `Archive/raw/`에 저장된 후 AI가 원본 내용을 수정·요약 덮어쓰기하지 않습니다.

원본 등록은 `Archive/AGENTS.md` §4를 따릅니다. 개인 생각은 `/brain-add-seed`로 연결하고, 혼합·불명확한 입력은 확인합니다. 동일 원본은 재사용하며 동명 다른 파일을 덮어쓰지 않습니다. 수신 Markdown 사본에만 5규약(`type: raw`, `status: unread` 등)을 넣고 본문은 보존합니다. 비문서·원형 보존 파일에는 frontmatter를 삽입하지 않습니다. URL을 읽지 못하면 읽은 척 저장하지 않고 본문이나 접근 가능한 파일을 요청합니다.

### 2. 기존 위키 확인 (`Archive/wiki/index.md` 선행 스캔)
`Archive/wiki/index.md`를 먼저 읽고 사용자에게 아래 2가지를 제시해 확인받습니다:
1. 이 자료에서 위키에 남길 **핵심 명제 1~3개**
2. **기존 위키 페이지 업데이트 vs 신규 페이지 생성** 판단

이 계획의 실제 N(현재 페이지)·K(신규 예정)를 세고 **`Archive/AGENTS.md` §3의 공통 규모 점검**을 적용합니다. 경고선에 닿는 신규 계획은 `/lint` 진단과 대안을 먼저 제시하고, 선택·승인받은 계획만 작성합니다. 이미 큰 위키의 기존 갱신은 숫자만으로 차단하지 않습니다.

### 3. `Archive/wiki/<카테고리>/` 페이지 작성·갱신
사용자 확인 후 `0_Me/_FRONTMATTER_REGS.md`에 등록된 카테고리 폴더(미존재 시 생성)에 저장·갱신합니다.

**Frontmatter (`0_Me/_FRONTMATTER_REGS.md` 5규약 필수):**
```yaml
---
type: wiki
created: YYYY-MM-DD
tags:
  - embedded # 0_Me/_FRONTMATTER_REGS.md 주제 태그 중 해당 태그
status: draft     # draft | organized | shared
source:
  - "[[Archive/raw/데이터시트-논문/파일명]]"
related:
  - "[[Archive/wiki/<카테고리>/연관 페이지|연관 페이지]]"
---
```

**본문 구조:**
```markdown
# [주제명]

## 핵심 요약
> 결론부터 1~2문장으로 핵심을 요약합니다.

## 핵심 명제 & 스펙
- [명제 또는 수치 1 — 근거/조건 포함]

## 상세 설명
[원리, 파라미터 계산, 동작 조건 등 — 추측 없이 원본 근거만 기록]

## 다른 관점 / 모순
- *(기존 위키 내용이나 다른 출처와 수치·관점이 충돌할 때만 ⚡ 표시와 함께 양쪽 출처를 병기)*

## 출처 및 연결
- 원본: [[Archive/raw/<유형>/<파일명>|<파일명>]]
- 내 노트/프로젝트: [[Ark/...]] 또는 [[Lab/Projects/...]] (있을 때만)
```

> **링크 충돌 방지:** `Ark/`와 카테고리명이 같으므로 위키링크는 `[[Archive/wiki/<카테고리>/<파일명>|<표시제목>]]` 전체 경로와 별칭을 사용합니다.

### 4. `Archive/wiki/index.md` & `Archive/log.md` 동시 갱신 (누락 금지)
1. **`Archive/wiki/index.md`:** 해당 카테고리 섹션에 `- [[Archive/wiki/<카테고리>/<파일명>|<제목>]] 📥 — 1줄 요약` 추가 및 총 위키 페이지 수·갱신일 수정
2. **`Archive/log.md`:** 하단에 `## [YYYY-MM-DD HH:MM] ingest | [제목]` 기록 append

위키·목차·로그를 재읽어 원본 참조와 저장 성공을 확인한 뒤에만 raw Markdown의 `status`를 `ingested`로 바꿉니다(`Archive/AGENTS.md` §4). 본문은 바꾸지 않으며 원형 보존 파일은 로그로 처리 상태를 남깁니다. 저장 실패 시 완료로 표시하지 않고, 재시도 시 기존 원본·위키·로그를 확인해 중복 생성·기록을 피합니다.

### 5. 완료 보고
- 생성·갱신된 위키 페이지 경로, `index.md` 및 `log.md` 반영 여부를 보고하고, 확인이 필요한 불확실한 수치·전제 조건을 짚습니다.

---

## 절대 하지 않는 것
- ❌ `Archive/raw/` 보존 없이 바로 `wiki/` 작성하기
- ❌ 원본 본문 수정·삭제, 승인 없는 이동, 동명 다른 파일 덮어쓰기 (등록·처리 상태 예외는 `Archive/AGENTS.md` §4)
- ❌ `Archive/wiki/index.md` 또는 `Archive/log.md` 갱신 누락
- ❌ 5규약(`type: wiki`, `tags`, `status: draft|organized|shared`, `source:`) 어기기
- ❌ 사용자 확인 없이 한 번에 여러 자료를 무더기로 자동 ingest하기
