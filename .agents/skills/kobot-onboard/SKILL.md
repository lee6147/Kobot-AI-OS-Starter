---
name: kobot-onboard
description: "신규 코봇 조원의 볼트(Kobot-AI-OS-Starter)를 첫 실행 시 자동으로 개인 맞춤형으로 구축합니다. `0_Me/ONBOARDING_STATUS.md`가 `completed: false`일 때, 또는 사용자가 \"/kobot-onboard\", \"온보딩 시작\", \"초기 세팅해줘\"라고 요청할 때 실행합니다."
---

# kobot-onboard — 조원 맞춤형 AI OS 자동 온보딩

> **목적:** 조원이 긴 강의 교안 `.md` 파일을 일일이 읽지 않아도, **대화 4단계만으로 내 전공·역할에 맞춘 폴더 구조, `0_Me/` 정의문, 5축 태그, `Archive/wiki/index.md`**를 자동으로 구축합니다.

## ⚙️ 경로 설정 (`Kobot-AI-OS-Starter` 전용)

| 이름 | 설정값 | 역할 |
|---|---|---|
| `STATUS_FILE` | `0_Me/ONBOARDING_STATUS.md` | 온보딩 완료 여부(`completed: false → true`) |
| `REGS_FILE` | `0_Me/_FRONTMATTER_REGS.md` | 5규약 및 본인 맞춤형 `tags` 허용 목록 |
| `TEMPLATES_DIR` | `_TEMPLATES_/` | `5S-자가진단 워크시트`, `BRAIN-Note`, `Daily-Note`, `Daily-Reflect`, `Source-Article`, `Template_Project`, `Template_Task`, `Template_Weekly-Review`, `자가진단-3축`, `30-90-365 체크리스트`, `Web-Clipper/*.json` 4종 |
| `ARK_DIR` | `Ark/` | 맞춤형 주제 방 3~5개가 생성될 자리 (`Blocks/` 유지) |
| `WIKI_DIR` | `Archive/wiki/` | `Ark/`와 1:1로 맞춘 위키 카테고리 폴더 및 `index.md` |

---

## 에이전트 실행 절차

### 1. 상태 확인
- `0_Me/ONBOARDING_STATUS.md`를 읽어 `completed: false`인지 확인합니다. 이미 `true`라면 *"이미 온보딩이 완료되어 있습니다. 주제 방이나 역할을 수정할까요?"*라고 먼저 묻습니다.

### 2. 4단계 1:1 인터뷰 (한 번에 1~2문항씩 질문 — 몰아 묻기 금지)
사용자가 부담 없이 답하도록 2턴에 나누거나 한 항목씩 묻습니다:

1. **기본 프로필:** "볼트에서 사용할 이름(닉네임)과 AI가 어떤 말투(예: 존댓말/간결형)로 답하면 좋을까요?"
2. **L1·L2·L3 맥락:** "올해 맡고 있는 역할(L2) 2~3개(예: 코봇 자율주행 팀원, 임베디드 학습자)와 이번 학기·분기 주력 프로젝트(L3) 1~2개는 무엇인가요?"
3. **핵심 지식 영역(3~5개):** "내가 남에게 자주 설명하거나 앞으로 계속 파고들 주제 3~5개는 무엇인가요? (기본 포함 추천: `기록-인사이트`, `독서`)"
4. **현재 기록 습관(5S 체크):** "평소 공부하거나 AI와 대화할 때 가장 아쉬운 점은 무엇인가요? (예: 대화만 하고 문서로 안 남김, 정리 없이 쌓아만 둠)"

### 3. 맞춤형 볼트 구조 제안 (승인 먼저)
답변을 바탕으로 아래 4가지를 표로 정리해 **사용자 확인·승인**을 받습니다:
- **`Ark/` & `Archive/wiki/` 1:1 폴더 목록 (3~5개):** 조원이 쓴 단어를 그대로 폴더명으로 살림
- **소문자 `tags` 3~5축 매핑:** 예) `ros2` (`ROS2-자율주행/`), `cpp` (`C++-알고리즘/`), `ai` (`AI-활용법/`), `record` (`기록-인사이트/`), `reading` (`독서/`)
- **첫 `Lab/Projects/` 후보 1개**
- **5S 진단 한 줄 처방**

### 4. 승인 후 자동 스캐폴딩 실행
사용자가 승인하면 아래 작업을 한 번에 실행합니다:
1. `Ark/<맞춤방>/`, `Ark/기록-인사이트/Daily/`, `Ark/기록-인사이트/Reviews/`, `Archive/wiki/<맞춤방>/`, `Outputs/블로그/`, `Outputs/카드뉴스/` 폴더 생성 (`Ark/Blocks/`는 유지)
2. `0_Me/Purpose-L1L2L3.md` 생성 (5규약 `type: me` + `L1 인생 목표 · L2 올해 역할 표 · L3 이번 분기 프로젝트 표` 구조로 직접 생성)
3. `0_Me/My-Knowledge-Domain.md` 생성 (5규약 `type: me` + `0단계 프라이밍 · 1단계 3~5개 지식영역+태그` 구조로 직접 생성)
4. `0_Me/5S-자가진단-YYYY-MM-DD.md` 생성 (`_TEMPLATES_/5S-자가진단 워크시트.md` 기반으로 4번 질문 결과 기록)
5. `0_Me/_FRONTMATTER_REGS.md`의 `나의 주제 태그 목록`을 조원의 확정 태그 3~5개로 교체
6. `Archive/wiki/index.md`에 확정된 3~5개 카테고리 섹션(`## 1. ...`) 및 1줄 설명 초기화
7. `Lab/Projects/<첫 프로젝트명>.md` 생성 (`_TEMPLATES_/Template_Project.md` 기반으로 승인된 첫 프로젝트 1개 자동 생성)
8. `AGENTS.md`와 `GEMINI.md`의 §1에 사용자 이름·역할 반영 (두 파일 해시 동일 유지)
9. `0_Me/ONBOARDING_STATUS.md`를 `completed: true`, `user_name`, `track: cli`, `domain_tags` 기입으로 갱신
10. `Archive/log.md`에 `## [YYYY-MM-DD HH:MM] init | [사용자명] 맞춤형 온보딩 완료` 기록

### 5. 첫 실전 행동 1개 즉시 유도
완료 보고 후, 개념 설명을 길게 늘어놓지 말고 바로 첫 행동을 제안합니다:
- *"세팅이 완료되었습니다! 1주차에는 딱 3가지(`/brain-add-seed`, `/ingest`, `/query`)만 쓰시면 됩니다. 지금 막 공부하신 내용 1줄이나 자료 1개만 던져 주세요. 첫 노트를 바로 만들어 드리겠습니다."*
- *"만약 개념 원리부터 순서대로 1:1 코칭을 받으며 실습을 완주하고 싶다면, Obsidian을 켜지 말고 바로 **`/kobot-guide`** (예: **\"Part 6부터 순서대로 실습 이끌어 줘\"**)라고 말씀해 주세요."*

---

## 절대 하지 않는 것
- ❌ 사용자 인터뷰 없이 임의로 폴더 5개 만들어 버리기 (⚠️ 함정 `#3` 남의 템플릿 복붙 방지)
- ❌ `AGENTS.md`와 `GEMINI.md` 중 한쪽만 수정하고 다른 쪽 누락하기
- ❌ 한 번에 10개가 넘는 방을 만들어 2레벨 단순성 깨기
