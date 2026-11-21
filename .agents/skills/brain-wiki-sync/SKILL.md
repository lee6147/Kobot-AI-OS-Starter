---
name: brain-wiki-sync
description: "`Ark/Blocks/`의 수확된 원자 블록(compiled_to 미기입)을 `Archive/wiki/<내 카테고리>/` AI 백과사전으로 컴파일하고 `index.md`와 `log.md`를 함께 갱신합니다. 사용자가 \"/brain-wiki-sync\", \"BWS\", \"내 지식을 위키에\", \"블록 위키로\"를 요청할 때 실행합니다."
---

# brain-wiki-sync — 내 원자 지식 → AI 백과사전(`Archive/wiki/`) 컴파일

> **핵심 원칙:** `Ark/Blocks/`의 블록은 원자 단위이고 `Archive/wiki/` 페이지는 **주제 단위**입니다. 블록 1개를 위키 1개로 1:1 복제하지 않고, 관련 블록들을 묶어 **기존 위키 페이지 갱신 우선 → 없을 때만 신규 생성**합니다.

## ⚙️ 경로 설정 (`Kobot-AI-OS-Starter`)

| 이름 | 설정값 | 설명 |
|---|---|---|
| `BLOCKS_DIR` | `Ark/Blocks/` | 입력 — 수확된 원자 블록 (`compiled_to` 비어 있는 항목) |
| `TOPICS_DIR` | `Ark/` | 원본 씨앗 추적용 (`source` 링크) |
| `WIKI_DIR` | `Archive/wiki/` | 출력 — 온보딩 시 생성한 카테고리 하위 폴더들 (`Ark/`와 1:1) |
| `WIKI_INDEX` | `Archive/wiki/index.md` | 위키 카탈로그 (`📥`/`💡`/`🔄` 마크 동기화) |
| `WIKI_LOG` | `Archive/log.md` | `Archive/` 작업 원장 |
| `PAGE_CAP` | `15` | 초기 위키 권장 상한 (`12` 이상 시 신규 생성 억제, `15+` 시 `/lint` 건강 점검 먼저) |

---

## 에이전트 실행 절차

### 0. 사전 점검 (실측 필수)
실행 전 반드시 3가지를 실제 파일 기준으로 셉니다:
1. **현재 위키 페이지 수:** `Archive/wiki/` 하위 카테고리의 실제 `.md` 수 (`AGENTS.md`, `index.md` 제외)
2. **전체 Block 수:** `Ark/Blocks/*.md` 수
3. **미컴파일 Block 목록:** frontmatter `compiled_to:`가 비어 있는 Block만 추출

### 1. 매핑 계획 제안 (**갱신 먼저, 신규는 나중**)
1. 미컴파일 Block들의 `tags`와 명제를 바탕으로 `Archive/wiki/index.md` 및 `Archive/wiki/<카테고리>/`를 먼저 검색합니다.
2. 이미 다루는 위키 페이지가 있으면 **기존 페이지 갱신** 후보(`📥` → `🔄`)로 올립니다.
3. 없을 때만 `0_Me/_FRONTMATTER_REGS.md` 태그-폴더 매핑에 맞는 카테고리에 **신규 위키 페이지**(`💡`)를 제안합니다. (대상 `Archive/wiki/<카테고리>/` 폴더가 아직 없으면 생성)
4. 내 Block의 주장이 기존 외부 자료 기반 위키 내용과 충돌하면 **모순 발견(`⚡`)**으로 명확히 보고합니다.

> **승인 필수:** 제안 표(`🔄 갱신` / `🆕 신규` / `⚡ 모순`)를 먼저 보여주고 사용자 확인 후 파일을 작성합니다.

### 2. 위키 페이지 작성 또는 갱신
**Frontmatter (`0_Me/_FRONTMATTER_REGS.md` 5규약 준수):**
```yaml
---
type: wiki
created: YYYY-MM-DD
tags:
  - embedded # 0_Me/_FRONTMATTER_REGS.md 주제 태그
status: draft     # draft | organized | shared
source:
  - "[[Ark/Blocks/<Block파일명>]]"
related:
  - "[[Archive/wiki/<카테고리>/<연관페이지>|<표시제목>]]"
---
```

**갱신 규칙:**
- 기존 위키 문장을 임의로 삭제하지 않고 **추가·통합**합니다.
- 외부 자료와 내 지식이 어긋나는 지점은 `## 다른 관점 / 모순` 섹션에 **`⚡` 표시와 함께 양쪽 출처(`📥` vs `💡`)를 나란히 병기**합니다.

### 3. `Archive/wiki/index.md` & `Archive/log.md` 동시 갱신
1. **`Archive/wiki/index.md`:**
   - **기존 `📥` 페이지에 내 지식이 합쳐진 경우:** 마크를 **`📥` → `🔄`로 승격**하고 1줄 요약을 보강합니다.
   - **Block만으로 새 페이지를 만든 경우:** 해당 카테고리에 `- [[Archive/wiki/<카테고리>/<파일명>|<제목>]] 💡 — 1줄 요약`을 추가하고 플레이스홀더를 지웁니다.
   - 상단의 `총 위키 페이지` 수와 `마지막 갱신` 날짜를 업데이트합니다.
2. **`Archive/log.md`:** 파일 하단에 아래 형식으로 append합니다:
```markdown
## [YYYY-MM-DD HH:MM] brain-wiki-sync | [페이지 제목]
- **출처 블록:** [[Ark/Blocks/...]] (총 N건)
- **결과:** [[Archive/wiki/<카테고리>/<파일명>]] (신규 💡 또는 갱신 🔄)
- **모순 병기:** [있으면 ⚡ 내용 1줄, 없으면 "없음"]
- **index.md 갱신:** 완료 (총 N페이지)
```

### 4. Block에 `compiled_to:` 표식 남기기 (중복 컴파일 방지)
- 컴파일된 각 `Ark/Blocks/*.md` 파일의 **본문은 건드리지 않고**, frontmatter의 `compiled_to:` 필드에만 대상 위키 페이지 경로를 기록합니다.

### 5. 완료 보고
- 신규/갱신 위키 수, 현재 용량(`N / 15`), `compiled_to` 표식 처리 건수, 발견된 모순(`⚡`) 요약을 보고합니다.

---

## 절대 하지 않는 것
- ❌ Block 1개를 위키 페이지 1개로 무작정 1:1 복제하기
- ❌ 기존 위키 페이지가 있는데 확인 없이 새 페이지 중복 생성하기
- ❌ `Building Block` 본문 내용 수정하기 (`compiled_to:` 필드만 기록)
- ❌ 기존 위키 문장 삭제하거나 모순(`⚡`)을 조용히 덮어쓰기
- ❌ `Archive/wiki/index.md` 또는 `Archive/log.md` 갱신 빠뜨리기
- ❌ `PAGE_CAP(15)` 초과 상태에서 `/lint` 건강 점검 없이 새 위키 페이지 늘리기
