---
type: me
created: 2026-11-16
tags:
  - record
status: organized
source: "[[_BOT_PACK_/KOBOT_STARTER_KNOWLEDGE_PACK]]"
---

# 📑 Frontmatter 5규약 (최소 표준 규약)

> 볼트 내 모든 노트 맨 위에 붙이는 5개 핵심 속성입니다.
> 완벽한 규칙이 아니라 검색과 AI 자동 분류를 작동시키는 최소 규약입니다.

## ✍️ 5규약 표

| # | 키 | 뜻 | 규약 |
|---|---|---|---|
| ① | `type` | 노트 종류 | 폴더와 1:1 — `me`(`0_Me`) · `memo`(`Welcome`) · `note`(`Ark`) · `wiki`(`Archive/wiki`) · `project`·`task`·`material`(`Lab`) · `output`(`Outputs`) |
| ② | `created` | 생성일 | `YYYY-MM-DD` 한 형식만 (예: `2026-11-16`) |
| ③ | `tags` | 연결 주제 | 온보딩 시 확정한 본인 `Ark/` 방과 맞춘 소문자 태그 최소 1개 (`#` 없이) |
| ④ | `status` | 진행 단계 | 지식·위키 `draft → organized → shared` · 프로젝트 `planning → in-progress → on-hold → done` · 태스크 `todo → doing → done` · 재료 `active → archived` |
| ⑤ | `source` | 출처 | 외부 자료 기반일 때만 URL, 강의명·회차 또는 `[[Archive/raw/...]]`. 순수 내 생각이면 비워 둠 |

## 🏷️ 나의 주제 태그 목록 (`tags` 허용값 — 온보딩 시 본인 전공·역할에 맞게 수정)

> 아래는 기본 예시입니다. `/kobot-onboard` 실행 시 본인 `Ark/` 폴더와 1:1로 맞춰 교체합니다.
- `ai` (`AI-활용법/`)
- `embedded` (`임베디드-제어/` 등 본인 주력 전공 태그로 변경)
- `project-study` (`프로젝트-스터디/` 등 본인 목표 태그로 변경)
- `record` (`기록-인사이트/`)
- `reading` (`독서/`)

## 🧩 Ark / Archive / Lab 확장 속성 (최소 보조 키)
- `stage`: `Ark/` 내 `BRAIN-Note`의 성숙도 (`seed` | `bloom` | `connected`) — `status`는 `draft`→`organized`→`shared` 유지
- `block_type`: `Ark/Blocks/` 원자 블록 유형 (`Concept` | `Argument` | `Counter` | `Model` | `Theory` | `Evidence`)
- `compiled_to`: `Ark/Blocks/` 블록이 `Archive/wiki/`로 컴파일되었을 때 대상 링크 (`[[Archive/wiki/...]]`)
- `related`: `Archive/wiki/` 교차 참조 위키링크 목록
- `lint_flag`: `/lint` 건강 점검(`180일` 기준) 시 검토가 필요한 페이지 표시 (`stale`)
- `Lab/` 실행 보조 키: `project`(`Lab/Tasks` 상위 프로젝트 링크), `due`, `est_hours`(`1~3`시간), `role`(`L2` 역할), `target_date`, `goal`, `material_role`(`reference|outline|checklist|asset-map`)
