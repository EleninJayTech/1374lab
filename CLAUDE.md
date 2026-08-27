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
- 기록(PROGRESS·DECISIONS)은 `docs/` flat. 마스터 PROJECT_PLAN.md도 docs/ 루트.
- /resume·/wrap은 대상을 PWD>브랜치>질문 순으로 판별.
- 기록 항목 양식: [갈래][상태(Done/Pending/Blocked/Reverted)] 설명 — @작성자 (브랜치) YYYY-MM-DD. 최상단 append. **갈래(값 = posts·en·ja·촬영배경 등 작업 축)·날짜를 비우지 않는다** — 날짜는 union 병합이 순서를 섞을 때 유일한 시간 근거다.

### 동시편집 충돌
- PROGRESS·DECISIONS는 append 전용(기존 줄 수정 금지). .gitattributes의 merge=union이 동시 append를 자동 합침.
- DECISIONS id는 동시 발번 충돌을 피해 날짜+작성자 포함(예: DEC-20260816-eleninjaytech).

### 여러 대상 동시 변경 (방법 A)
- 주 대상 docs에 본문, 함께 바뀐 대상엔 [공통] 교차 한 줄 + 링크.
- 다른 repo는 접근 가능(cwd 또는 additionalDirectories)하면 자동 교차기록, 불가하면 "○○에 기록 필요" 알림.

### 세션 워크플로 규율
- 작업 요청엔 **성공 기준·검증 방법**을 함께 받는다. 이 repo는 자동 테스트가 없으므로 검증은 **브라우저로 해당 페이지를 열어 확인**하고 그 결과를 증거로 보고한다 — "됐다"는 말로 끝내지 않는다.
- append-only(과거 수정·삭제 금지). 결정이 바뀌면 새 DEC + 기존에 "Superseded by DEC-…" 표시. 상호참조는 [[DEC-…]]·날짜.
- /resume: **remote 있고 워킹트리 clean이면 `git pull --ff-only` 먼저**(아니면 `git fetch` 후 뒤처짐만 보고 — 병합은 사용자 결정) → **git status·브랜치로 미커밋(진행 중) 작업 발견** → 그다음 PROGRESS 최상단(**최근 ~5항목만** — 항목이 길면 항목당 앞 ~700자에서 끊는다. 줄 수가 아니라 **항목 수**다, 통째 읽기 금지) + PROJECT_PLAN 현재 Phase + 최근 DEC 3건. 읽은 `[Pending]`·`[Blocked]`는 **PROJECT_PLAN "미해결/관찰 중"과 대조**해 거기 없으면 이미 닫힌 것으로 본다 — 종결은 PROJECT_PLAN에서만 일어나고 PROGRESS 마커는 영구히 남는다.
- /wrap: PROGRESS append + 새 DEC + PROJECT_PLAN 체크박스 갱신 + **미커밋이면 경고**(커밋 전엔 다음 /resume가 git status로만 발견).
- **미해소 `[Pending]`·`[Blocked]`는 PROJECT_PLAN "미해결/관찰 중"에 한 줄로 올려 닫힐 때까지 유지**한다. PROGRESS는 append 전용이고 /resume은 최근 ~5항목만 읽으므로, 그 항목이 5항목 창 밖으로 밀리는 순간 어느 절차도 다시 보지 않는다.
- 정기 항목(재검증·점검 주기)은 PROJECT_PLAN에 **`다음 ○○: YYYY-MM경`** 형식으로 남긴다 — /resume의 "예정일 경과" 안내가 읽는 형식이라, 안 적으면 그 안내는 발화하지 않는다. 반대로 그 안내는 **미완료 항목만** 읽는다 — 줄 머리가 `- [x]`이거나 제목이 취소선(`~~…~~`)이면 예정일이 남아 있어도 제외한다. 그래서 항목을 닫을 때 예정일 문자열을 지울 필요가 없다(지우면 이력이 사라진다).

### 길이 관리
- PROGRESS·DECISIONS가 약 800줄 **또는 약 120KB**(먼저 닿는 쪽)를 넘으면 가장 오래된 항목부터 docs/archive/로 옮기고 활성 파일 **최상단(제목 바로 아래) 포인터 한 줄**("이전 기록: docs/archive/…"). **얼마나 자를지는 달력이 아니라 목표치로** — 활성 파일이 **임계의 절반 이하**가 될 때까지(가능하면 날짜 경계에 맞춰). 맨 아래가 아니라 최상단인 이유 — /resume가 최근 ~5항목만 읽어 아래 포인터는 보이지 않는다. 이후 append는 **포인터 줄 아래부터**. **과거 이력 검색은 활성 파일이 아니라 `docs/` 폴더 단위로**(아카이브 자동 포함).

### 공유 vs 개인 / 시크릿
- 공유(커밋): CLAUDE.md·.claude/settings.json·.gitattributes·docs/.
- 개인(커밋 금지): .claude/settings.local.json·CLAUDE.local.md.
- 시크릿(.env·키·비밀번호)은 읽지도 커밋하지도 않는다.

### 토큰 참고
- docs/는 자동 선로딩되지 않고 필요 시만 읽힌다. 루트 CLAUDE.md는 항상 로드되므로 짧게 유지.

<!-- dropin-applied: 2026-08-28(이 줄은 **현재 상태**만 — 경위는 git 히스토리) · 모드=전체 · 언어=ko · 00 v1.35 · 01 v1.56(**§B 단일 repo(B형)** — §F-1 기록 규칙 v1.56 기준 동기[최근 ~5항목·PROJECT_PLAN 대조·예정일 미완료 필터·800줄/120KB 2축·갈래+날짜]·산출물 언어 한 줄) · 03 v1.20(디자인 스킬은 글로벌 frontend-design·svg-design — 동명 그림자 방지로 프로젝트 설치 안 함. MCP 해당 없음[.mcp.json 없음], 브라우저 검증=Claude in Chrome) · 04 v1.16(§1 hooks 해당 없음—빌드·린트·테스트 명령이 없는 정적 HTML repo / §2 CLI는 PC 스코프 — 글로벌 기록 참조) · 미선택: 02(해당 없음—비개발 repo, 02 STEP 1 가드), 04 CI(해당 없음—PR 워크플로 미사용), 05, 06 · 출처=클론 D:\claude -->
