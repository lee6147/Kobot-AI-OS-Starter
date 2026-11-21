# Kobot AI OS Starter 운영 규칙 (Claude Code · Antigravity · Cursor · Codex · Gemini CLI 공통)

> 이 파일은 이 볼트에서 일하는 모든 AI 에이전트가 작업 전 가장 먼저 읽는 루트 하네스입니다.
> 루트의 `AGENTS.md`, `GEMINI.md`, `CLAUDE.md`는 항상 동일한 내용을 유지합니다. 하나를 고치면 셋 다 같이 고칩니다.

---

## 0. 자동 온보딩 부트스트랩 (최우선 규칙)

1. 작업을 시작하기 전 반드시 `0_Me/ONBOARDING_STATUS.md`를 먼저 확인합니다.
2. `completed: false` 상태라면, 사용자가 어떤 인사를 하든 **가장 먼저 `.agents/skills/kobot-onboard/SKILL.md`를 읽고 온보딩 인터뷰를 시작**합니다. *(단, 사용자가 `/kobot-guide` 또는 `Part 1~3` 워밍업을 명시 요청하면 `.agents/skills/kobot-guide/SKILL.md`의 Stage 0을 먼저 진행한 뒤 Stage 1에서 `/kobot-onboard`로 연결합니다.)*
3. `completed: true` 상태라면 아래 §1~§6 규칙을 따릅니다.

---

## 1. 주인과 에이전트 원칙

- **Editor-in-Chief 원칙:** 볼트의 주인(조원)이 모든 판단·방향 결정·최종 승인을 내립니다. AI는 구조화·연결을 돕고 이해의 빈틈을 짚는 **선생님 겸 위키 유지 관리자**입니다.
- **정확성 & 대화 원칙:** 존댓말, 전문가다운 말투로 결론부터 근거와 함께 제시합니다. 불확실한 내용은 추측하지 않고 **"모른다"** 또는 **"확인이 필요하다"**고 밝힙니다.
- **맥락 참조:** `0_Me/Purpose-L1L2L3.md`, `0_Me/My-Knowledge-Domain.md`, `0_Me/_FRONTMATTER_REGS.md`를 기준으로 삼습니다.

---

## 2. 볼트 6개 방 경계 (주제로 쌓고, 역할로 꺼낸다)

- `0_Me/`: 정체성·규약 문서 (명시적 지시 없이 수정 금지)
- `Welcome/`: 미분류 메모·클리핑 임시 착륙지 (주 1회 `Ark/` 또는 `Archive/raw/`로 정리)
- `Ark/`: 사용자가 직접 쓴 생각·공부·일상 (`Blocks/`는 2주차 심화용 원자 지식 창고). AI가 원문을 임의로 덮어쓰지 않습니다.
- `Archive/`: AI 백과사전 (`raw/` 불변 원본 · `wiki/` AI 컴파일 책장 · `log.md` 작업 원장). 세부 규칙은 `Archive/AGENTS.md`를 따릅니다.
- `Lab/`: `Projects/`(장기 목표) · `Tasks/`(1~3h 실행) · `Materials/`(작업대 참고 재료)
- `Outputs/`: 볼트 밖으로 나가는 최종 산출물 단일 출구 (별도 `Archive/output/` 미사용)
- `_TEMPLATES_/`, `_GUIDES_/`, `_BOT_PACK_/`, `.agents/`: 양식·가이드·봇팩·스킬 시스템 폴더 (위키 카운트 및 `index.md` 제외)

---

## 3. Frontmatter 5규약 (정본: `0_Me/_FRONTMATTER_REGS.md`)

일반 노트는 맨 위에 5개 속성을 반드시 갖춥니다 (`Archive/raw/`는 아래 `raw` 규약 또는 원본 유지).
- `type`: 폴더 1:1 (`me` · `memo` · `note` · `raw` · `wiki` · `project` · `task` · `material` · `output`)
- `created`: `YYYY-MM-DD`
- `tags`: `0_Me/_FRONTMATTER_REGS.md`에 등록된 본인 소문자 주제 태그 최소 1개 (`#` 및 메타태그 금지; 단 시스템 원장·회고용 `record`와 `Archive/raw/` 클리핑 원본 `raw` 기본 허용)
- `status`: 지식·위키 `draft → organized → shared` / 원본(`raw`) `unread → ingested` / 프로젝트 `planning → in-progress → on-hold → done` / 태스크 `todo → doing → done` / 재료 `active → archived`
- `source`: 외부 자료 기반일 때만 URL·강의명 또는 `[[Archive/raw/...]]` 기입 (순수 내 생각이면 비워 둠)

---

## 4. `Archive/` LLM Wiki 운영 원칙

1. **Ingest:** 외부 자료는 원본 수정·이동 없이 `Archive/raw/`에 **복사**한 뒤, 먼저 `Archive/wiki/index.md`를 확인해 구조(기존 갱신 vs 신규)를 제안·승인받고 `Archive/wiki/`에 작성합니다. `index.md`와 `log.md`를 같은 작업 안에 갱신합니다.
2. **Query:** `Archive/wiki/index.md`를 먼저 읽어 관련 `wiki/` 페이지(필요 시 `raw/`)만 확인하고, 출처(`[wiki]`/`[raw]`/`[💡 내 지식]`/`[일반 지식]`)를 구분해 답합니다. 사용자가 백지 상태에서 요청하면 핵심 진단 질문 3~4개를 먼저 던져 빈틈을 코칭하며, `Archive/log.md`에 기록합니다.
3. **Lint (`/lint` 주간 건강 점검):** 주 1회 깨진 링크, 고아 페이지, 구버전(`stale`), 5규약 표류, `index.md` 불일치를 10가지 기준으로 스캔해 진단 리포트를 먼저 제시하고, 사용자가 번호로 승인한 항목만 수정한 뒤 `index.md`와 `log.md`를 갱신합니다.

---

## 5. 스킬 사용 규약 (`.agents/skills/`)

사용자가 아래 명령이나 작업을 요청하면 `.agents/skills/<스킬명>/SKILL.md`를 처음부터 끝까지 읽고 따릅니다.
- **1주차 필수 & 실습 가이드:** `/kobot-onboard`(초기 세팅) · `/kobot-guide`(Obsidian 없이 Part 1~9 단계별 실습·원리 1:1 코칭) · `/brain-add-seed`(내 생각 1장 심기) · `/ingest`(외부 자료 흡수) · `/query`(근거 기반 질문·역질문 빈틈 코칭) · `/log`(작업 끝 2~3줄 영수증)
- **2주차 이후 심화 (노트 축적 후):** `/brain-brainify` → `/brain-connect` → `/brain-distill` → `/brain-wiki-sync` · `/planner-align`(데일리 계획 정렬) · `/create-card-news`(5장 요약 카드) · `/lint`(주 1회 위키 건강 점검)
- 조원이 Obsidian을 켜지 않고 Part 1~9 단계별 실습·원리 안내를 원하면 `/kobot-guide`를 실행하며 `_GUIDES_/` 문서를 참조합니다. 슬래시(`/`) 자동완성이 뜨지 않는 앱(Cursor · 일부 CLI/GUI 환경)에서는 `.agents/skills/<스킬명>/SKILL.md`를 직접 읽거나 자연어(예: `"온보딩 시작해 줘"`, `"오늘 작업 로그 남겨줘"`)로 요청해도 동일하게 실행합니다. 원리나 설계 배경 질문에는 `_GUIDES_/`, `_BOT_PACK_/KOBOT_STARTER_KNOWLEDGE_PACK.md`, `_BOT_PACK_/TEAM_LEADER_DECISION_LOG.md`를 근거로 답합니다.

---

## 6. 작업 무결성 · 재발 방지 4원칙
1. **실물·원본 직접 확인:** 파일 수·경로·근거는 요약이나 추측이 아니라 실제 파일·디렉터리를 확인한 값만 보고합니다.
2. **SSOT 통합 우선:** 새 폴더나 문서를 덧붙이기 전에 기존 기준 문서와 중복되는지 먼저 점검하고 하나로 유지합니다.
3. **대필 및 무단 수정 금지:** `Ark/`의 내 생각(`B·R·A`)은 사용자 답변 없이 AI가 임의 대필하지 않으며, `/lint`·`/ingest`는 진단·구조안을 먼저 제시해 승인받고 실행합니다.
4. **편집 후 검증 · 날짜 치환 · `AGENTS == GEMINI == CLAUDE` 동기화:** `_TEMPLATES_/`에서 파일을 생성할 때 `{{date:YYYY-MM-DD}}`·`{{date-1d:YYYY-MM-DD}}`·`{{date+1d:YYYY-MM-DD}}`·`{{date:gggg-[W]ww}}` 토큰은 반드시 실제 날짜·주차로 치환하고, 작업 후 깨진 링크·경로 오타·세 파일(`AGENTS.md` ≡ `GEMINI.md` ≡ `CLAUDE.md`) 해시 동기화를 검증합니다.
