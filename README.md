# Kobot AI OS Starter (코봇 AI 연구팀 지식 체계 · Part 1~9 완결판)

> **긴 강의 파일들을 일일이 찾아 읽거나, 처음부터 Obsidian 설정과 씨름할 필요가 없습니다.**
> 팀장이 Part 1~9 전 과정을 먼저 완주하며 부딪힌 **오류 10가지를 구조적으로 예방**해 두었습니다.

---

## 코봇 팀 추천 2단계 운영 전략

1. **1단계 · 구축 및 실습 (`Obsidian 최소화 · 로컬 폴더 에이전트 창 하나로 완주`):**
   - 이 저장소를 내려받은 뒤 **Obsidian을 계속 켜두지 않아도** 로컬 폴더를 직접 여는 AI 앱(**Antigravity · Claude Code · Cursor · Gemini CLI · Codex** = 트랙 A) 창 하나만으로 온보딩부터 Part 9 실습까지 자동 저장하며 끝낼 수 있습니다.
   - *(단, 브라우저 **Web Clipper**를 쓰려면 처음 1회만 Obsidian에서 이 폴더를 열어 볼트를 등록해야 URI 연동이 정상 작동합니다. 일반 채팅 앱인 **Grok · ChatGPT(웹/데스크톱) · 웹 Gemini · 일반 Claude 채팅**은 로컬 파일 쓰기 권한이 없으므로 아래 **트랙 B** 복붙 방식으로 진행합니다.)*
2. **2단계 · 매일 일상 활용 (`Grok 에이전트 중심 · 높은 가시성`):**
   - 실습과 볼트 뼈대가 완성된 뒤에는 `_BOT_PACK_` 2파일과 내 프로필 요약을 **Grok Project(에이전트)** 등에 올려 매일 할 일·위키 조회·저녁 Daily Review를 가시성 높게 굴립니다.
   - *(중급자 권장 · `Clip 8.1.1`)* 노트가 **15~20개 이상** 쌓이면 월 1회만 Obsidian 그래프 뷰(4렌즈)를 열어 AI가 놓친 연결점을 내 눈으로 직접 점검하세요.

---

## 30초 시작하기 (트랙 A · B 중 택 1)

### 트랙 A · 로컬 폴더 접근 AI 에이전트 (Antigravity · Claude Code · Cursor · Gemini CLI · Codex)
1. 이 저장소를 `git clone`(또는 `Code` → `Download ZIP`)합니다. *(주의: 조원은 `lee6147/Kobot-AI-OS-Starter`에 푸시 권한이 없으며, 본인 개인 기록 `0_Me/`·`Ark/`를 공유 원격 `origin`에 올리지 마세요. 온보딩으로 개인화한 `AGENTS.md`·`GEMINI.md`·`CLAUDE.md`에도 목적·자료 경로·루틴이 들어가므로 공개 저장소나 PR에 올리지 마세요. 원격 백업이 필요하면 본인 Private 레포를 새 원격으로 연결해 사용하세요.)*
2. **Obsidian을 켜지 말고** 이 폴더를 작업 경로로 AI 앱을 연 뒤, **바로 던지는 첫 마디 3가지** 중 하나로 시작하세요:
   1. **`"온보딩 시작"`** (또는 `/kobot-onboard`) — 내 역할·주제 방·5규약 태그 자동 세팅 *(최초 1회 권장)*
   2. **`"파트 1부터 차근차근 진행하자"`** (또는 `/kobot-guide`, `"Part 4부터 순서대로 실습하자"`) — Part 1~3 워밍업(`Before-Diagnosis.md`)부터 Part 9 졸업까지 1:1 단계별 실습 코칭. 받은 답변·다음 질문·승인 대기를 저장하므로 새 채팅에서는 `"이어서 하자"`로 재개합니다.
   3. **`"지금 공부한 거 정리해 줘 / 빈틈 체크해 줘"`** — *(온보딩 완료 후)* `/brain-add-seed`·`/ingest`·`/query`로 즉시 정리 및 역질문 코칭 *(미온보딩 시 자동으로 온보딩부터 안내)*
   *(슬래시 `/` 자동완성이 뜨지 않는 앱에서는 `"AGENTS.md 읽고 온보딩 시작해 줘"`나 `"오늘 작업 로그 남겨줘"`처럼 자연어로 요청하면 `.agents/skills/`를 읽어 동일하게 동작합니다.)*
3. **Obsidian 1회 열기 예외 vs AI 창 대체법 구분:**
   - **예외 (처음 1회만 열기):** 브라우저 **Obsidian Web Clipper**(`_TEMPLATES_/Web-Clipper/*.json` 4종)를 쓰려면 처음 1회만 Obsidian에서 이 폴더를 열어 볼트를 등록해야 URI 연동이 작동합니다.
   - **대체법 (Obsidian 0회):** Web Clipper를 쓰지 않고 **URL이나 기사·문서 텍스트를 AI 채팅창에 바로 붙여넣어** `/ingest` 또는 `Archive/raw/` 저장을 요청하면 Obsidian을 한 번도 열지 않고 똑같이 완주할 수 있습니다.
4. **1주차 필수 명령:** `/kobot-onboard`(초기 세팅) · `/kobot-guide`(Part 1~9 코치) → `/brain-add-seed`(내 생각), `/ingest`(외부 자료), `/query`(빈틈·역질문 코칭), `/log`(작업 끝 2줄 영수증).
5. **2주차 이후 심화:** `/brain-brainify` → `/brain-connect` → `/brain-distill` → `/brain-wiki-sync` · `/planner-align` · `/create-card-news` · `/lint`.

### 트랙 B · 일반 채팅 / 웹 AI 조원 (Grok · ChatGPT 웹/데스크톱 · 웹 Gemini · 일반 Claude 채팅)
1. **`Code` → `Download ZIP`**으로 압축을 풉니다. *(트랙 B 앱은 내 PC 폴더에 파일을 직접 저장할 수 없으므로, 봇이 뽑아 주는 완성 `.md` 코드블록을 내 로컬 폴더에 저장·복붙하는 방식으로 진행합니다.)*
2. 사용 중인 AI에서 봇/프로젝트를 1개 만들고 `_BOT_PACK_/`의 **딱 2개 파일**만 넣습니다:
   - **지침(Instructions) 복붙:** `_BOT_PACK_/BOT_SYSTEM_PROMPT.md`
   - **지식(Knowledge) 첨부:** `_BOT_PACK_/KOBOT_STARTER_KNOWLEDGE_PACK.md` *(Gemini Gem · Grok Project · ChatGPT Custom GPT/Project · Claude Project 동일; 선택: Part 1~9 원리까지 깊게 코칭받으려면 `_GUIDES_/Part1~3-핵심정리.md`, `Part4~9-핵심정리.md`도 함께 업로드 권장)*
3. 채팅창에 **"온보딩 시작"**이나 **"Part 1부터 단계별로 같이 실습하자"**를 치거나, 공부 메모·음성 1분 덤프를 던지면 저장 폴더·파일명·완성 `.md` 파일을 바로 출력해 줍니다.

---

## 📦 포함된 Part 1~9 핵심 자산
- **`.agents/skills/` (13종):** `kobot-onboard` · `kobot-guide`(Part 1~9 단계별 1:1 실습 코치) · `brain-*` 5종 · `ingest` · `query` · `lint` · `planner-align`(볼트 내부 완결형) · `log`(2층 단독 표준) · `create-card-news`
- **`_GUIDES_/` (4종):** `Part1~3-핵심정리.md` · `Part4~9-핵심정리.md` · `가이드-①-터미널익숙버전.md` · `가이드-②-터미널부담버전.md` (Obsidian 없이도 원리와 실습 순서를 바로 따라가는 핵심 가이드)
- **`_TEMPLATES_/` (15종):** `Before-Diagnosis`(Part 1~3 통합 스냅샷) · `5S-자가진단` · `BRAIN-Note` · `Daily-Note`(2단 리뷰) · `Daily-Reflect` · `Source-Article` · `Template_Project` · `Template_Task` · `Template_Weekly-Review` · `자가진단-3축` · `30-90-365 체크리스트` · `Web-Clipper/*.json` 4종(`raw-book`·`raw-datasheet-paper`·`raw-lecture-video` 3종 `https://` 도메인 트리거, `raw-article`은 전 웹페이지 수동 실행 `triggers: []` & `저장 이유/반응` 헤딩 포함)
- **`_BOT_PACK_/`:** [`KOBOT_STARTER_KNOWLEDGE_PACK.md`](_BOT_PACK_/KOBOT_STARTER_KNOWLEDGE_PACK.md) (7대 함정·Part 1~9 원리) · [`TEAM_LEADER_DECISION_LOG.md`](_BOT_PACK_/TEAM_LEADER_DECISION_LOG.md) (오류 예방 · D1~D15 의사결정 · 재발 방지 회고 로그) · [`BOT_SYSTEM_PROMPT.md`](_BOT_PACK_/BOT_SYSTEM_PROMPT.md)
