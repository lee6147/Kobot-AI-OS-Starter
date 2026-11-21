---
name: brain-distill
description: 성숙한 `Ark/` BRAIN Note에서 재사용 가능한 원자 명제를 6 Building Blocks(Concept·Argument·Counter·Model·Theory·Evidence)로 추출해 `Ark/Blocks/`(flat)에 수확합니다. 사용자가 "/brain-distill", "블록 추출", "원자화해줘", "Blocks로 수확"을 요청할 때 실행합니다.
---

# brain-distill — 원자 지식 수확하기 (`Ark/` → `Ark/Blocks/`)

> **핵심 원칙:** 원본 `BRAIN Note`는 `Ark/<내 주제 방>/`에 그대로 남기고, 여러 프로젝트와 산출물(`Outputs/`)에서 **반복 재사용할 수 있는 원자 명제 1~3개만** `Ark/Blocks/`(flat)로 수확합니다.

## ⚙️ 경로 설정 (`Kobot-AI-OS-Starter`)

| 이름 | 설정값 | 설명 |
|---|---|---|
| `TOPICS_DIR` | `Ark/` | 입력 `BRAIN Note`가 있는 내 주제 방들 |
| `BLOCKS_DIR` | `Ark/Blocks/` | 출력 원자 블록 저장소 (**flat 유지, 하위 폴더 금지**) |
| `TEMPLATE` | `_TEMPLATES_/BRAIN-Note.md` | 입력 노트의 섹션 구조 기준 |

### 섹션 지도 — 어디에서 어떤 블록을 추출하나
| 입력 노트 섹션 | 캐낼 재료 | 대표 `block_type` |
|---|---|---|
| `## 요약` | 핵심 한 문장 명제 | `Concept` / `Argument` |
| `## 1. 개요` | 출처 사실·측정 조건 | `Evidence` / `Concept` |
| `## 2. 생각` → `Resonate` | 강하게 공명한 핵심 명제 | `Argument` |
| `## 2. 생각` → `Amplify` | 심층 추론 / 예외·반박 | `Argument` / `Theory` / `Counter` |
| `## 3. 활용` → `Integrate` | 개념 간 관계 / 대조 긴장 | `Model` / `Counter` |
| `## 3. 활용` → `Navigate` | 검증할 가설·시사점 | `Theory` |

---

## 6 Building Blocks 분류표
| `block_type` | 핵심 질문 | 예시 |
|---|---|---|
| **`Concept`** | "이건 무엇인가?" | `Concept-부트스트랩 플로팅 전원.md` |
| **`Argument`** | "왜 그렇게 설계하는가?" | `Argument-수집만으로는 내 문제가 정의되지 않는다.md` |
| **`Counter`** | "언제 틀리거나 한계가 오나?" | `Counter-100% 듀티에서는 부트스트랩 충전이 유지되지 않는다.md` |
| **`Model`** | "구조·관계가 어떻게 작동하나?" | `Model-터미널 친숙도별 AI 교육 2트랙.md` |
| **`Theory`** | "어떤 조건을 바꾸면 결과가 달라지나?" | `Theory-게이트 저항 감소와 서지 전압의 트레이드오프.md` |
| **`Evidence`** | "실제 데이터시트·실험 수치는 무엇인가?" | `Evidence-PWM 스위칭 손실 실측 데이터.md` |

---

## 에이전트 실행 절차

### 1. 중복 수확 점검 & 대상 노트 분석 (선행 필수)
1. 먼저 `Ark/Blocks/` 전체의 `source:` 속성을 스캔해 **이미 이 노트에서 수확된 블록이 있는지 확인**합니다. (있으면 목록을 먼저 보고해 중복 추출 방지)
2. 대상 `Ark/` 노트를 읽고 추출 가능한 원자 블록 후보 **1~3개**를 `BlockType: "명제"` 형식으로 사용자에게 제안합니다.

### 2. 사용자 선택 확인 후 `Ark/Blocks/` 파일 생성
승인된 항목만 `Ark/Blocks/`(미존재 시 폴더 생성)에 `<BlockType>-<명제>.md`로 생성합니다 (`Counter-argument`가 아닌 `Counter` 사용).

**Frontmatter (`0_Me/_FRONTMATTER_REGS.md` 5규약 준수):**
```yaml
---
type: note
created: YYYY-MM-DD
tags:
  - embedded # 0_Me/_FRONTMATTER_REGS.md에 등록된 내 주제 태그 중 선택
status: organized # 수확된 원자 블록 기본 상태 (Outputs 전파 시 shared)
source: "[[Ark/<방>/<원본 BRAIN Note>]]"
block_type: Concept # Concept | Argument | Counter | Model | Theory | Evidence
compiled_to:        # /brain-wiki-sync 전까지 비워 둠
---
```

**본문 구조:**
```markdown
# [BlockType] — [명제 제목]

## 한 줄 정의
> [이 블록이 주장하는 핵심 1문장]

## 핵심 근거 (1~3개)
- [전제 → 결론 또는 수치·사실 근거]

## 한계 · 반론 (틀릴 수 있는 조건)
- [예외 조건이나 트레이드오프]

## 적용 · 시사점
- [Lab/Projects 또는 Outputs에서 어떻게 쓸 것인가]

## 연결
- 원본 씨앗: [[Ark/<방>/<원본 BRAIN Note>]]
- 인접 블록/위키: [[...]]
```

### 3. 양방향 링크 반영
- 원본 `BRAIN Note`의 frontmatter `related:`(및 필요 시 `### Integrate (연결) → 🟢 동조하는 생각`)에 새로 만든 `[[Ark/Blocks/<BlockType>-<명제>]]` 링크를 추가합니다.

### 4. 완료 보고
- 생성된 블록 경로 목록과 중복 방지 확인 결과를 보고하고, 미컴파일 블록이 **3~5개 이상** 쌓이면 `/brain-wiki-sync` 실행을 안내합니다.

---

## 절대 하지 않는 것
- ❌ 중복 스캔 없이 이미 수확한 명제 또 만들기
- ❌ 사용자 선택 없이 후보 4~6개 전부 생성하기
- ❌ `type: building-block`이나 `status: distilled`로 5규약 어기기 (`type: note`, `status: organized`, `block_type:` 사용)
- ❌ `Ark/Blocks/` 아래에 하위 폴더 만들기 (flat 유지)
- ❌ 원본 `BRAIN Note` 삭제·이동하기
