---
name: create-card-news
description: 볼트의 노트(BRAIN Note · `Archive/wiki` 페이지) 또는 완성 초안(`Outputs/` 긴 글)을 5장 요약 카드뉴스 + 공유 캡션 형식으로 변환합니다. 사용자가 "/create-card-news", "이걸 카드뉴스로", "카드뉴스 5장", "요약 카드로 접어줘" 등을 요청할 때 트리거.
---

# create-card-news — 노트·초안 → 5장 카드뉴스 (`Kobot-AI-OS-Starter` 공용)

> 📤 파이프라인 위치: **Part 8 출력 라인 (`8.2.2` — Asset #9).**
> *"지식은 하나, 그릇은 여러 개."* 쌓아둔 `Ark/` 노트·`Archive/wiki/`·`Outputs/` 긴 글을 팀 전파·스터디 공유·포트폴리오용 **5장 요약 카드 + 공유 캡션**으로 재편집합니다.

## ⚙️ 경로 설정 (`Kobot-AI-OS-Starter` 표준)

| 이름 | 설정값 | 설명 |
|---|---|---|
| `OUTPUT_DIR` | `Outputs/카드뉴스/` | 카드뉴스 착지점 (`YYYY-MM-DD - [주제] 카드뉴스.md`) |
| `BLOG_DIR` | `Outputs/블로그/` | 긴 글 초안 (유형 B 입력원) |

## 에이전트 실행 절차
1. **입력 분석 & 후크 3종 먼저 제안:** 지정 노트(유형 A: `Ark`/`wiki` 또는 유형 B: `Outputs` 긴 글)를 읽고, 바로 5장을 쓰지 않고 **Card 1 후크 후보 3가지**(A. 명제형 / B. 질문형 / C. 대비·숫자형)를 먼저 제안해 사용자가 고르게 합니다.
2. **5장 본문 작성 (카드당 3~6줄, 15초 호흡):**
   - `Card 1 (Cover 후크)` → `Card 2 (문제·배경)` → `Card 3 (핵심 원리·구조)` → `Card 4 (실전 적용·체크리스트)` → `Card 5 (결론 한 줄 & 다음 걸음)`
   - 원문에 없는 과장된 주장이나 근거 없는 수치는 절대 지어내지 않습니다.
3. **공유 캡션 작성:** 메신저·노션·SNS 공유용 2~3문장 + 해시태그 5~7개 포함.
4. **저장 (`type: output` 5규약 준수):** `OUTPUT_DIR`(미존재 시 자동 생성)에 `YYYY-MM-DD - [주제] 카드뉴스.md`로 저장하고 5규약(`type: output`, `created: YYYY-MM-DD`, `tags: [...]`, `status: draft`, `source: "[[원본 노트 경로]]"`)을 정확히 채웁니다.
