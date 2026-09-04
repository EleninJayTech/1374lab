# CLAUDE.md — 1374 연구소 블로그

> 프로젝트 **사실**만 적는다. 행동 규칙은 글로벌 `~/.claude/CLAUDE.md`가 담당(중복 금지).

## 무엇인가
생활용품 실사용 후기 정적 블로그 + 영상 촬영용 배경 웹. GitHub Pages 배포(`https://eleninjaytech.github.io/1374lab/`).

## 스택 — 빌드가 없다
**손으로 쓴 정적 HTML**이다. Jekyll·Hugo 같은 생성기를 쓰지 않는다(`_config.yml`·`package.json` 없음).
- 포스트 1건 = 폴더 하나 + 그 안의 `index.html`(+ `og.png`).
- 스타일은 각 HTML의 인라인 `<style>` — 공통 CSS 파일이 없다. 브랜드 컬러 `#E86A33`, 본문 `#1A1A1A`, 폰트 Pretendard/Noto Sans KR.
- 빌드·테스트·린트 명령이 **없다**. 검증은 브라우저로 직접 연다.

## 구조
```text
index.html                     # 한국어 홈(포스트 목록)
posts/<YYYY-MM-DD-slug>/       # 한국어 포스트
en/index.html, en/posts/...    # 영어
ja/index.html, ja/posts/...    # 일본어
001_Dish Soaps/                # 영상 촬영 배경 웹(블로그와 별개 산출물)
feed.xml, sitemap.xml, robots.txt
```

## 포스트를 추가·수정할 때 함께 고칠 것 (수동 동기화 — 빠뜨리기 쉬움)
빌드 도구가 없어 **아무것도 자동으로 갱신되지 않는다**. 새 포스트 1건마다:
1. `posts/<slug>/index.html` 생성(+ `og.png`)
2. 해당 언어 홈(`index.html` / `en/index.html` / `ja/index.html`) 목록에 링크 추가
3. `sitemap.xml`에 `<url>` 추가 — 다국어판이 있으면 **세 언어 모두에 `xhtml:link hreflang` 3줄**을 서로 걸어준다(한 곳만 빠져도 짝이 깨진다)
4. `feed.xml`에 `<item>` 추가
5. `<link rel="canonical">`이 그 페이지 자신의 URL을 가리키는지 확인

## 커밋 양식
`post: <slug> (<lang>)` — 기존 이력을 따른다(예: `post: 2026-07-09-freezer-portioning (ja)`). 그 외 변경은 `<타입>: <요약>`.

---

## 문서·기록 규칙 (Claude가 자동 적용)

> **산출물 언어 = 한국어**(00 STEP 3 `언어=ko` · 01 §F-1) — 아래 규칙이 규정하는 산출물(`CLAUDE.md` 본문·`docs/` 기록 항목·커밋 메시지·산출물 파일명)은 한국어로 쓴다. 단 **형식 키워드는 언어와 무관하게 고정**이다: `[Done]`·`[Pending]`·`[Blocked]`·`DEC-YYYYMMDD-nn`·`dropin-applied`. `/resume`·`/wrap`이 이 문자열로 항목을 찾으므로, 번역하면 어느 절차도 그 항목에 걸리지 않는다.

### 작업 기록 구조
- 기록(PROGRESS·DECISIONS)은 flat(docs/) 또는 단위 분할(docs/<단위>/, MSA 정책). 마스터 PROJECT_PLAN.md만 docs/ 루트.
- /resume·/wrap은 대상(repo+단위)을 PWD>브랜치>질문 순으로 판별.
- 기록 항목 양식: [갈래][상태(Done/Pending/Blocked/Reverted)] 설명 — @작성자 (브랜치) YYYY-MM-DD. 최상단 append. **갈래·날짜를 비우지 않는다** — 다인 공유 파일에서 갈래는 /resume의 유일한 필터고, 날짜는 union 병합이 순서를 섞을 때 유일한 시간 근거다.

### 동시편집 충돌
- PROGRESS·DECISIONS는 append 전용(기존 줄 수정 금지). .gitattributes의 merge=union이 동시 append를 자동 합침.
- **PROJECT_PLAN.md·CLAUDE.md엔 merge=union을 걸지 않는다** — 체크박스 토글·기록 줄 교체는 줄 수정이라 union이면 양쪽이 다 남아 파손된다. 충돌은 손으로: PROJECT_PLAN은 체크박스=완료 우선·미해결=합집합, CLAUDE.md의 dropin-applied는 적용일 최신 줄 기준.
- **되돌림**: 원항목을 지우지 말고 [Reverted] 새 항목 append + 원항목 끝에 "→ Reverted <날짜>". git revert가 PROGRESS 줄까지 지우면 그 줄은 되살린다(기록은 코드와 함께 되돌리지 않는다).
- DECISIONS id는 동시 발번 충돌을 피해 날짜+작성자 포함(예: DEC-20260703-min).

### 여러 대상 동시 변경 (방법 A)
- 주 대상 docs에 본문, 함께 바뀐 대상엔 [공통] 교차 한 줄 + 링크.
- 다른 repo는 접근 가능(cwd 또는 additionalDirectories)하면 자동 교차기록, 불가하면 "○○에 기록 필요" 알림. **그 알림은 나에게만 뜬다** — 상대에게 도달하는 채널은 그 repo의 git뿐이니, 주 대상 기록에 영향받는 repo·대상을 명시하고 **그 변경의 PR·이슈 본문에도 한 줄** 남긴다(개별 repo에서 각자 여는 팀에선 이 경로가 기본값이다).

### 세션 워크플로 규율
- 작업 요청엔 **성공 기준·검증 명령**(테스트/빌드/재현 스크립트)을 함께 받는다. 구현 후 그 검증을 실행해 **증거(출력)로 보고** — "됐다"는 말로 끝내지 않는다.
- append-only(과거 수정·삭제 금지). 결정이 바뀌면 새 DEC + 기존에 "Superseded by DEC-…" 표시. 상호참조는 [[DEC-…]]·날짜.
- /resume: **remote 있고 워킹트리 clean이면 `git pull --ff-only` 먼저**(아니면 `git fetch` 후 뒤처짐만 보고 — 병합은 사용자 결정) → **git status·브랜치로 미커밋(진행 중) 작업 발견** → 그다음 PROGRESS 최상단(**최근 ~5항목만** — 길면 항목당 앞 ~700자. 줄 수가 아니라 항목 수다, 통째 읽기 금지. **상단 날짜가 역순이면 날짜 기준**으로 다시 고른다 — 장수 통합 브랜치 병합 뒤엔 최상단이 최근이 아니다) + PROJECT_PLAN(현재 Phase·**미해결/관찰 중** — 인계 항목이 여기 있다) + 최근 DEC 3건. **다인 공유 파일이면 최상단 5항목이 남의 갈래로 채워진다** — 현재 브랜치·PWD가 가리키는 갈래를 우선 읽고 나머지는 갈래별 한 줄로 요약한다.
- /wrap: PROGRESS append + 새 DEC + PROJECT_PLAN 체크박스 갱신 + **미커밋이면 경고**(커밋 전엔 다음 /resume가 git status로만 발견).
- **미해소 `[Pending]`·`[Blocked]`는 PROJECT_PLAN "미해결/관찰 중"에 한 줄로 올려 닫힐 때까지 유지**한다. 닫는 것은 PROJECT_PLAN에서만 일어나므로 PROGRESS 마커는 남는다 — `/resume`은 그 마커를 **PROJECT_PLAN과 대조**해 거기 없으면 닫힌 것으로 본다(안 그러면 닫힌 항목을 매번 열린 것으로 보고한다). PROGRESS는 append 전용이고 /resume은 최근 ~5항목만 읽으므로, 그 항목이 5항목 창 밖으로 밀리는 순간 어느 절차도 다시 보지 않는다.
- 정기 항목(재검증·점검 주기)은 PROJECT_PLAN에 **`다음 ○○: YYYY-MM경`** 형식으로 남긴다 — /resume의 "예정일 경과" 안내가 읽는 형식이라, 안 적으면 그 안내는 발화하지 않는다. 반대로 그 안내는 **미완료 항목만** 읽는다(`[x]`·취소선으로 종결된 항목은 제외) — 종결 시 예정일 문자열을 지우지 않아도 되고, 지우면 이력이 사라진다.
- **다음 세션이 이어받을 것**(`docs/plans/` 계획서 경로·다음 행동)은 **PROGRESS 항목 끝에 두지 않는다** — 항목 **앞쪽**에 적고, 세션 이후에도 유효하면 **PROJECT_PLAN**에 한 줄로 올린다. 항목 끝은 /resume의 **700자 컷**에 잘려 도달하지 않는다(경로·다음 계획을 끝에 붙이는 관행이 흔해 실제로 잘린다 — 파일은 남는데 가리키는 손가락만 잘린다). 다음 행동이 **없으면 없다고 적는다** — 안 적으면 다음 세션이 미해결 목록에서 재구성하게 되고, 그 재구성이 맞았는지 확인할 방법이 없다.

### 길이 관리
- PROGRESS·DECISIONS가 약 800줄 **또는 약 120KB**(먼저 닿는 쪽)를 넘으면 가장 오래된 항목부터 docs/archive/로 옮기고 활성 파일 **최상단(제목 바로 아래) 포인터 한 줄**("이전 기록: docs/archive/…"). 두 축인 이유 — 1항목=1줄로 적는 저장소는 줄 수가 임계의 4분의 1일 때 이미 100KB를 넘는다. **얼마나 자를지는 달력이 아니라 목표치로 정한다** — 활성 파일이 **임계의 절반 이하**가 될 때까지(가능하면 날짜 경계에 맞춰). 분기·월 같은 달력 단위는 부피와 무관해서, 그 단위로 잘라도 임계 아래로 못 내려가면 매 세션 다시 발화한다. 맨 아래가 아니라 최상단인 이유 — /resume가 최근 ~5항목만 읽어 아래 포인터는 보이지 않는다. 이후 append는 **포인터 줄 아래부터**(포인터 최상단 고정). **과거 이력 검색은 활성 파일이 아니라 `docs/` 폴더 단위로**(아카이브 자동 포함) — 파일만 검색하면 "기록 없음"으로 오판한다.

### 공유 vs 개인 / 시크릿
- 공유(커밋): CLAUDE.md·.claude/skills·.claude/settings.json·.gitattributes·docs/·.mcp.json(팀 MCP 서버 — 각자 첫 실행 때 승인).
- 개인(커밋 금지): .claude/settings.local.json·CLAUDE.local.md. 개인 노트는 auto-memory(~/.claude/projects/<proj>/memory, 머신 로컬·200줄/25KB).
- 시크릿(.env·키·비밀번호)은 읽지도 커밋하지도 않는다.

### 토큰 참고
- docs/는 자동 선로딩되지 않고 필요 시만 읽힌다. 분할은 토큰이 아니라 정리·타겟 정확도용.

<!-- dropin-applied: 2026-09-04 · 모드=전체 · 언어=ko · 00 v1.43 · 01 v1.66(§B 단일 repo(B형) **신규 설치** — §F-1 블록·§F-3 settings·§F-4·§F-5를 전부 문서 템플릿에서 재생성[누적 커스텀 제거]. 글로벌 §D는 글로벌 기록 참조) · 03 v1.27(디자인 스킬은 글로벌 `frontend-design`·`svg-design` — 동명 그림자 방지로 프로젝트 설치 안 함. MCP 해당 없음[.mcp.json 없음], 브라우저 검증=Claude in Chrome) · 04 v1.21(§1 hooks 해당 없음—빌드·린트·테스트 명령이 없는 정적 HTML repo / §2 CLI는 PC 스코프 — 글로벌 기록 참조) · 미선택: 02(해당 없음—비개발 repo, 02 STEP 1 가드), 04 CI(해당 없음—PR 워크플로 미사용), 05, 06 · 출처=클론 D:\claude-dev -->
