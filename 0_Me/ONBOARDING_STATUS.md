---
type: me
created: 2026-11-16
tags:
  - record
status: draft
source:
---

# 🚦 온보딩 상태 (Onboarding Status)

- completed: false
- user_name: (미설정 — `/kobot-onboard` 실행 시 자동 기입)
- track: (track-a | track-b)
- domain_tags: []
- guide_stage: not-started (not-started | stage-0 | stage-1 | stage-2 | stage-3 | stage-4 | stage-5 | graduated)
- guide_step: none

> **에이전트 안내:** `completed: false`이고 `guide_stage: not-started`인 초기 상태에서는, `AGENTS.md` §0에 따라 먼저 **"Part 1~3 워밍업(`Stage 0`)부터 차근차근 시작할까요, 아니면 바로 맞춤 폴더 세팅(`/kobot-onboard`)부터 할까요?"**를 1줄로 확인해 선택된 순서대로 진행하십시오. 각 단계·스텝이 끝날 때마다 `guide_stage`와 `guide_step`을 함께 갱신해 다음 대화에서도 정확히 이어갈 수 있게 유지합니다.
