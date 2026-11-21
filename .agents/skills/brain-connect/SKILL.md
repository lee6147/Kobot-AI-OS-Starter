---
name: brain-connect
description: "`Ark/` 내 BRAIN Note의 `## 3. 활용` 섹션(Integrate·Navigate)을 채워 볼트 내 실재하는 다른 노트·블록·위키·프로젝트와 연결합니다. 사용자가 \"/brain-connect\", \"연결점 찾아줘\", \"I/N 채워줘\", \"볼트 전체 스캔\"을 요청할 때 실행합니다."
---

# brain-connect — 지식 별자리 연결하기 (`bloom → connected`)

> **핵심 원칙:** 새 노트를 만들지 않습니다. 실제 검색으로 확인된 **실재하는 파일**만 링크하며, 대상 노트의 `## 3. 활용`만 제자리에서 채웁니다. (`요약` · `1. 개요` · `2. 생각`은 건드리지 않습니다.)

## ⚙️ 경로 설정 (`Kobot-AI-OS-Starter`)

| 이름 | 설정값 | 역할 |
|---|---|---|
| `TOPICS_DIR` | `Ark/` | 내 주제 방 (`Blocks/` 제외) |
| `BLOCKS_DIR` | `Ark/Blocks/` | 수확된 원자 블록 (flat) |
| `WIKI_DIR` | `Archive/wiki/` | AI 컴파일 위키 |
| `PROJECTS_DIR` | `Lab/Projects/` | 🔵 적용 가능한 실행 프로젝트 연결 |
| `EXCLUDE` | `_TEMPLATES_/`, `_BOT_PACK_/`, `.agents/`, `Welcome/`, `AGENTS.md`, `GEMINI.md`, `CLAUDE.md` | 검색 제외 대상 |

### 섹션 지도 (`_TEMPLATES_/BRAIN-Note.md` 1:1 매칭)
| 연결 유형 | 노트 안 위치 |
|---|---|
| 🟢 **동조하는 생각** | `### Integrate (연결)` → `- 🟢 **동조하는 생각:**` |
| 🟡 **대조되는 생각** | `### Integrate (연결)` → `- 🟡 **대조되는 생각:**` |
| 🔵 **적용 가능한 곳** | `### Integrate (연결)` → `- 🔵 **적용 가능한 곳 (Lab/Projects):**` |
| 🧭 **다음 탐색** | `### Navigate (다음 한 걸음)` → `다음에 읽을 것` / `다음에 쓸 것` / `미해결 질문` |

---

## 에이전트 실행 절차 (단일 노트 모드)

### 1. 대상 노트 읽기
- `/brain-connect [[노트]]`로 지정된 `Ark/` 노트를 먼저 읽고 핵심 명제를 파악합니다.

### 2. 실제 볼트 검색 (추측 금지)
- `TOPICS_DIR`, `BLOCKS_DIR`, `WIKI_DIR`, `PROJECTS_DIR`의 파일명과 본문을 실제로 검색합니다. (`EXCLUDE` 대상 제외)
- 존재하지 않는 파일명을 지어내 링크하지 않고, 아직 없는 미래 노트는 `🎯 미작성: [노트 주제]` 텍스트로만 적습니다.

### 3. 4가지 연결 후보 제안 → 승인 후 `## 3. 활용` 업데이트
- 🟢 동조 / 🟡 대조 / 🔵 적용 프로젝트 / 🧭 다음 탐색 후보를 근거 1줄과 함께 제시하고 사용자 승인 후에만 채웁니다.
- `Archive/wiki/` 링크는 `[[Archive/wiki/<카테고리>/<파일명>|<표시제목>]]` 전체 경로+별칭으로 기록합니다.

### 4. 메타데이터 갱신 (`0_Me/_FRONTMATTER_REGS.md` 준수)
- `stage: bloom` → **`stage: connected`**
- `status: draft` → **`status: organized`**
- `related:`(frontmatter)에는 🟢·🟡 핵심 연결 링크 **2~4개만** 추가합니다. (`tags:`에 메타 상태 태그 추가 금지)

---

## 🌐 전체 스캔 모드 (`/brain-connect 볼트 전체 스캔`)
1. `Ark/` · `Ark/Blocks/` · `Archive/wiki/` · `Lab/Projects/` 전체에서 제목·`tags`·요약 줄을 훑습니다.
2. 서로 다른 주제 방을 잇는 `cross-topic` 후보를 우선순위로 **5~10개** 제안합니다 (`노트 A ↔ 노트 B · 근거 1줄`).
3. **"왜 연결되는가(1문장) / 양방향 모두 의미 있는가 / 연결로 무엇을 할 수 있는가"** 3질문을 통과하고 사용자가 채택한 후보만 반영합니다.

---

## 절대 하지 않는 것
- ❌ 실재하지 않는 노트명으로 가짜 `[[wikilink]]` 만들기
- ❌ 사용자 확인 없이 연결 후보 자동 저장하기
- ❌ 대상 노트의 `요약` · `1. 개요` · `2. 생각` 내용 건드리기
- ❌ `status: connected`처럼 5규약 `status` 필드에 `connected` 직접 넣기
