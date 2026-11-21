---
name: planner-align
description: 데일리 노트의 "오늘의 계획" 항목을 `Lab/Tasks/` 태스크 노트 및 데일리 노트의 "시간 블록" 표와 양방향 정렬한다. 대응하는 Task 노트가 있으면 연결·갱신하고, 없으면 `Template_Task.md` 규격으로 생성한다. 사용자가 "/planner-align", "오늘 계획 정렬해줘", "데일리 노트 태스크로 연결", "오늘 시간표 짜줘" 등을 언급할 때 트리거.
---

# planner-align — 오늘의 계획을 `Lab/Tasks` · 시간 블록으로 정렬하기 (`Kobot-AI-OS-Starter` 공용)

> 📅 파이프라인 위치: **Part 7 수집·정리 4축 중 세 번째 — 할 일·일정 정렬.**
> 외부 앱(Todoist · Google Calendar) 의존 없이 볼트 내부만으로 100% 완결됩니다. (시간 블록은 생활 리듬에 맞춰 선택 생략 가능)

## ⚙️ 경로 설정 (`Kobot-AI-OS-Starter` 표준)

| 이름 | 설정값 | 설명 |
|---|---|---|
| `DAILY_NOTE_DIR` | `Ark/기록-인사이트/Daily/` | 데일리 노트가 사는 곳 (`YYYY-MM-DD.md`) |
| `PLAN_HEADING` | `## 오늘의 계획` | 사람이 먼저 오늘 할 일을 적는 섹션 |
| `TIMEBLOCK_HEADING` | `## ⏰ 시간 블록` | 시간표 배치 섹션 (선택 생략 가능) |
| `TASKS_DIR` | `Lab/Tasks/` | 태스크 노트가 사는 곳 (`type: task`) |
| `PROJECTS_DIR` | `Lab/Projects/` | 상위 프로젝트 검색 위치 (`type: project`) |
| `TEMPLATE_TASK` | `_TEMPLATES_/Template_Task.md` | 신규 태스크 생성 양식 |
| `TEMPLATE_DAILY` | `_TEMPLATES_/Daily-Note.md` | 데일리 노트 생성 양식 |

## 에이전트 실행 절차
1. **데일리 노트 확인:** `DAILY_NOTE_DIR/{오늘 YYYY-MM-DD}.md`를 읽습니다. 없으면 확인 후 `_TEMPLATES_/Daily-Note.md`로 생성하되, Obsidian 없이 생성하므로 **`{{date:YYYY-MM-DD}}`·`{{date-1d:YYYY-MM-DD}}`·`{{date+1d:YYYY-MM-DD}}` 토큰은 반드시 실제 오늘·어제·내일 날짜로 치환**합니다. **`## 오늘의 계획`이 비어 있으면 AI가 임의로 채우지 않고 멈춥니다 (함정 `#6` 순서 역전 금지).**
2. **항목 파싱 & 중복 방지:** `- [ ] …` 항목 중 이미 `%%[aligned]%%` 마커가 붙은 줄은 건너뜁니다.
3. **기존 `Lab/Tasks/` 매칭:** 키워드로 기존 `todo`/`doing` 태스크를 찾아 사용자 확인 후 연결·`due` 갱신.
4. **신규 태스크 생성 (프로젝트성 항목만):** 상위 프로젝트(`Lab/Projects/`)·완료 조건(Done 기준)·`est_hours`(1~3h)를 확인한 뒤 `_TEMPLATES_/Template_Task.md` 규격으로 생성합니다. 단발성 잡일("택배 부치기" 등)은 `Lab/Tasks/`를 만들지 않고 데일리 노트에만 유지합니다.
5. **시간 블록 배치 (선택):** 시작 시각은 절대 지어내지 않고 묻습니다. 사용자가 생략을 원하면 비워 둡니다.
6. **비파괴 되쓰기:** 사용자가 적은 원래 문장은 수정하지 않고 줄 끝에만 `%%[aligned]%% [[Lab/Tasks/...]]`를 이어붙입니다.

## ❌ 안 하는 것
- `## 오늘의 계획`이 비어 있는데 AI가 임의로 채우기
- 사용자가 쓴 계획 문장을 임의로 수정·덮어쓰기
- 상위 프로젝트·완료 조건 확인 없이 신규 태스크 생성하기
- 시간 블록 시작 시각을 임의로 지어내기
