# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 프로젝트 성격

주 4회 2분할 운동 기록 웹앱. **단일 `index.html`**(약 2,190줄)에 데이터·스타일·로직이 모두 들어 있고 빌드 도구·패키지 매니저·테스트 러너·번들러가 전혀 없다. 외부 런타임 의존은 Firebase SDK(ESM CDN 동적 import)와 정보 팝업의 YouTube iframe 뿐이다.

- 라이브: https://nisloke.github.io/workout-app/ (GitHub Pages, `main` 루트 서빙 → **push = 배포**)
- 리포: `nisloke/workout-app` (public)

## ★ 앱은 여기가 정본, 프로그램 내용은 저기가 정본 (가장 중요)

`index.html`은 **앱의 단일 정본**이다(2026-08-19, 정정대장 #19로 일원화 — 그 전에는 `rework\운동앱-주4회-2분할.html`이 정본이고 이 파일이 복사본이었다. 옛 절차를 설명하는 문서를 만나면 그쪽이 낡은 것이다). 하지만 **앱이 표시하는 프로그램 내용의 정본은 다른 곳에 있다.**

| 층 | 정본 경로 | 무엇의 정본인가 |
|---|---|---|
| 프로그램 내용 | `C:\Users\User\Documents\workout-program\rework\프로그램-주4회-2분할.md` | 종목·세트·반복·휴식·시간·주간 볼륨 |
| 앱 구현 | `workout-app\index.html` (이 파일) | HTML·CSS·JS·`#appdata` 전량 |

작업 절차:

1. **프로그램 수치(종목·세트·반복·휴식·시간)를 바꾸려면 앱을 먼저 고치지 말고 MD 수정을 먼저 제안한다.** 앱은 MD를 화면으로 옮긴 것이지 재설계 지점이 아니다. MD 수정이 승인되면 그다음에 앱 `#appdata`를 맞춘다.
2. 앱 UI·로직 변경은 `index.html`을 직접 편집하고 커밋·푸시한다(= 배포). 다른 곳에 사본을 만들지 않는다.
3. 사용자 지시로 프로그램·앱이 바뀌면 `rework\정정대장.md`에 한 행(번호·일자·지시·반영 범위·상태)을 추가하는 것이 이 프로젝트의 관례다. 앱 전용 변경은 "앱 반영(프로그램 내용 변경 없음)"으로 적는다.

이 앱의 근거 자료는 **전부 `C:\Users\User\Documents\workout-program\`** 에 있다. 어떤 파일이 무엇인지는 아래 "자료 폴더" 절.

## 공개 경계 (넘지 말 것)

| 레포 | 가시성 | 내용 |
|---|---|---|
| `nisloke/workout-app` (이 리포) | **PUBLIC** + Pages가 `main` 루트 서빙 | 배포 산출물만 |
| `nisloke/workout-program` | **PRIVATE** | 근거 자료 전량(전사 인용·논문 조사·개인 신체 데이터) |

이 리포에 커밋한 파일은 `https://nisloke.github.io/workout-app/<경로>` 로 **누구나 받아갈 수 있다.** 그래서 `workout-program`의 자료(ATHLEAN-X 자막 전문, P1 전사 인용 688KB, 체중·수행 기록이 든 sp/FINAL 문서)를 이 리포로 옮기거나 복사하지 않는다. 레포를 private으로 바꾸는 것도 해법이 아니다 — 무료 플랜은 private 레포 Pages가 안 되어 사이트가 내려가고, Pro여도 Pages 사이트 자체는 공개 URL로 서빙되므로 파일 노출은 그대로다(접근 제어 Pages = Enterprise Cloud 전용).

## 자료 폴더 — `C:\Users\User\Documents\workout-program\` (= private 레포 `nisloke/workout-program`)

이 폴더에는 **서로 다른 두 계보**가 섞여 있다. 구분하지 못하면 무효 처리된 설계를 근거로 앱을 고치게 된다.

- **SP 라인**(폴더 루트, 2026-08-07~09): 논문 조사 → 설계 → 적대 검증으로 이어진 선행 작업. 결과물의 종목 20개 중 13개가 지정 재료가 아니라 사용자의 과거 프로그램(`powerbuilding-ppl`)에서 유래한 것이 드러나 **설계 산출물이 무효 처리**됐다. 논문 조사 노트층은 살아 있으나 종목·서열 근거로는 못 쓴다.
- **rework 라인**(`rework\`, 2026-08-09~): 그 재작업이자 **현행 정본**. 지금 앱이 표시하는 모든 내용의 출처다.

### `rework\` — 현행 정본과 그 원장

| 파일 | 역할 |
|---|---|
| `프로그램-주4회-2분할.md` | **프로그램 정본**. §7.1·7.3 진행 규칙, §5 모빌리티 대본, §3 세션표. 앱 수치는 이것과 기계 대조로 일치해야 한다 |
| `운동앱-정본-위치.md` | 앱 HTML이 이 리포로 옮겨졌다는 포인터(정정대장 #19). 옛 `운동앱-주4회-2분할.html` 자리 |
| `정정대장.md` | 사용자 정정·지시 원장(현재 19건). 앱 기능이 왜 그렇게 생겼는지의 1차 근거 |
| `PROMPT.md` | 재작업 지시서. §1 소스 규칙(아래)·§5 금지사항 |
| `PROMPT-HTML.md` | 앱 요구사항 정본. §2 지시 12건·§3 이전 구현에서 실증된 함정·§4 납품 검증 |
| `P1-종목후보표.md` | **전사 인용 원장**(688KB). 종목별 `[전사: 채널·영상ID]` 인용과 `P1 (a)#n` 앵커. 큐·주의사항 문구의 출처 확인은 여기 |
| `P1-청크분담표.md` | 전사 4파일 46청크 전수 스캔 커버리지 기록(405편) |
| `근육별-티어리스트.md` | 앱 `option.tier`/`score`(15점 만점)의 채점 근거 |
| `P3-검증보고.md` | 정본 MD·P1 표에 대한 독립 적대 검증(인용 실재성 정규화 매칭) |
| `athleanx-perfect-series-en.txt` | 1차 소스 4파일 중 하나(아래) |
| `firebase-setup.md` | 이 리포 것과 동일 사본 |

### 폴더 루트 — SP 라인 (선행 조사·설계, 열람 제한 있음)

`sp0`(현행 앱 실측) · `sp1`(채널 신규영상 갭) · `sp2a~sp2d2`(ACSM 2026 Position Stand, 종목 A vs B 장기 RCT 전수 스캔, 능동 ROM·갭, 처방 파라미터·인용 감사) · `sp3a`(3채널 주장 카탈로그, 판정 없음) · `sp3b1/3b2`(주장↔논문 대조 판정 — 모빌리티군/훈련군) · `sp4·4b·4c`(종목 평가표·커버리지·세트 계상 규약) · `sp5·5r·5x·5y·5z`(설계서·개정판·정적 스트레칭 3부작) · `sp6a/6b`(적대적 검증·인용 감사) · `FINAL-workout-program.md`(SP 라인 최종본, 12주 A1/B1/A2/B2) · `workout-app.html`(이전 앱 구현 — `PROMPT-HTML.md` §3 "실증된 함정"의 출처).

### 소스 규칙 (`rework\PROMPT.md` §1 — 위반 시 산출물 무효)

프로그램 내용(종목·구조·원칙)을 새로 만들거나 고칠 때 적용된다. 앱의 표시 문구·큐를 손볼 때도 근거를 여기서 찾아야 한다.

**1차 소스 = 종목·구조·원칙의 유일한 출처 (전사 4파일)**

```
C:\Users\User\Documents\ichon_yoga_scripts_merged.txt                                     이촌동요가원 136편
C:\Users\User\Documents\awesomebliss\transcripts_clean_full.txt                           어썸블리스 198편
C:\Users\User\Documents\rapid-overload-workout-guide\deliverables\급진적_과부하_전체자막_통합_2026-08-05.txt   65편
C:\Users\User\Documents\workout-program\rework\athleanx-perfect-series-en.txt             ATHLEAN-X 6편(영어)
```

경로가 틀리면 **탐색하지 말고 정지·보고**한다(대체 파일을 임의로 고른 것이 지난번 사고의 입구였다). 전사 의미검색은 `search-workout` 스킬(workout-rag)로도 가능하다.

- **2차 소스(논문)**: 전사에서 나온 주장·수치의 검증과 파라미터 보강 전용. 논문이 종목을 새로 들여오는 입구가 되면 안 되고, 논문 커버리지 비대칭을 서열 근거로 쓰지 않는다.
- **금지**: `C:\Users\User\Documents\powerbuilding-ppl\` 일체(열지도, 기억으로 재구성하지도 않는다). `workout-program\` 하위는 **기본 전부 금지의 화이트리스트 방식** — 예외는 `rework\` 하위와 조건부 열람 파일(`sp1`·`sp2*`·`sp3a`·`sp3b1/3b2`)뿐이며, 그 밖(`sp0`·`sp4*`·`sp5*`·`sp6*`·`FINAL-*`·구 `workout-app.html`)을 열어야 하면 먼저 승인을 받는다.
- 조건부 파일도 **P1 후보표 확정 이후, 편성 확정 종목의 파라미터 검증에만** 쓴다. 종목 선택·서열 근거로는 금지(오염된 목록 기준으로 수집됨). sp 계열은 한국어 3채널만 다루므로 ATHLEAN-X 대조에는 쓸 수 없다.
- 서브에이전트를 쓰면 이 소스 규칙 전문 + 대상 파일 절대경로를 각 프롬프트에 복사해 넣는다(최상위에만 두면 하위가 자기 입력만 보고 다른 재료를 쓴다 — 이전 실패의 기계적 원인).

## 명령

빌드·린트·테스트 없음. 확인은 브라우저 실측이 전부다.

```powershell
# 로컬 확인 — file:// 로 열지 말 것 (localStorage 유실·Firebase auth 도메인 불가)
python -m http.server 8000
# → http://localhost:8000/index.html , 모바일 뷰포트 약 390px 로 확인

# 배포 (push = 배포. 사본 동기화 단계는 없다)
git add index.html; git commit -m "..."; git push

# 근거 자료 레포도 함께 바뀌었으면 (정정대장·정본 MD 등)
git -C C:\Users\User\Documents\workout-program add -A; git -C C:\Users\User\Documents\workout-program commit -m "..."; git -C C:\Users\User\Documents\workout-program push
```

검증 체크리스트(납품 기준, `PROMPT-HTML.md` §4): 앱 표시 수치 ↔ 정본 MD 기계 대조 불일치 0 / 콘솔 JS 에러 0 / 아래 "바꾸면 안 되는 설계 결정" 전수 확인.

## 파일 구조 (index.html 내부)

| 줄 범위 | 내용 |
|---|---|
| 7–177 | CSS. 라이트 기본 + `@media (prefers-color-scheme: dark)` + `:root[data-theme="dark"]` 3중 정의 (토글이 양방향으로 이기게) |
| 179–285 | HTML 골격. 메인 화면 + `#recordsView`(기록 오버레이) + `#dayModal`(날짜 기록) + `#infoModal`(종목 정보) + `#toast` |
| 287–1120 | `<script id="appdata" type="application/json">` — **프로그램 데이터 전량** |
| 1122–2190 | IIFE 앱 로직 (`"use strict"`, 프레임워크 없음, 순수 DOM) |

## 데이터 스키마 (`#appdata`)

```
{ sessions: { A|B: { days, label, gym, strength, total, slots[] } },
  mobility|warmup|cardio: { totalSec, rows[{t, sec, name, how}], place?, note? } }
```

`slots[]` 원소는 두 종류:
- `type:"ex"` — `{num, rest, blocks:[1개]}`
- `type:"ss"`(슈퍼세트) — `{num, label:"S1", rounds, rest, blocks:[2개]}`

`block` = `{id, muscle, scheme, ramp?, options[]}`, `option` = `{id, name, tier, score, cue, why, warn, yt}`.

- `scheme`은 **문자열이 스펙이다.** `parseScheme()`이 정규식 `/램프 (\d+)/`·`/(\d+)×(\d+)~(\d+)/`로 파싱해 세트 체크박스 개수와 증량/감량 판정 임계값을 만든다. 표기를 바꾸면 판정이 조용히 죽는다(`ps.max == null` → 판정 없음).
- `block.id`의 **첫 글자 `a`/`b`가 세션을 결정한다**(`scoreByDate()`의 `key.charAt(0) === "b"`). 새 블록 id는 반드시 해당 세션 접두어로.
- `option.yt`는 YouTube Shorts ID. `youtube-nocookie.com/embed/`로 **팝업을 열 때만** iframe 생성.

## 저장 스키마와 기록 키 규약

`localStorage["wp4_v1"]`에 통째 JSON:

```
{ sel: {blockId: optionId},        // 드롭다운 선택 기억
  rec: {"blockId::optionId": [{d:"YYYY-MM-DD", w, r, m, u, c[]}]},
  units: {"blockId::optionId": "kg"|"lb"},
  daily: {"YYYY-MM-DD": {bw, sl}}, // 체중·수면
  ui: {session, date, theme, savedAt} }
```

- **기록 키는 반드시 `blockId::optionId`.** 종목 id 단독 키는 한 세션의 두 슬롯에 같은 종목을 고르면 기록이 뭉개진다(실증된 함정).
- `w`/`r`/`m`은 문자열 그대로 저장, `u`는 단위, `c`는 세트 체크 boolean 배열(램프 포함).
- `save()`는 `ui.savedAt = Date.now()` 갱신 → `localStorage` 기록 → `schedulePush()`(1.5초 디바운스 클라우드 푸시)를 **항상 이 순서로** 한다. 저장 경로를 우회해 `localStorage`에 직접 쓰면 동기화가 끊긴다(예외: `pullCloud()`가 원격 승리 시 의도적으로 직접 씀).
- 입력은 **자동 저장**이다. 저장 버튼이 없고 `input` 이벤트마다 `upsertEntry()`가 돈다. 무게·횟수·메모가 모두 비고 체크도 없으면 그날 엔트리를 삭제하고, 배열이 비면 키 자체를 지운다.

## 기록 대상 날짜 (`recDate`)

빠뜨린 종목을 나중에 채워 넣을 수 있도록, 입력 화면 전체가 **활성 날짜 하나**를 바라본다(정정대장 #20).

- `recDate`가 기록 대상 날짜다. 기본은 오늘이고 `isPastMode()`는 `recDate !== todayStr()`.
- **`recDate`는 저장하지 않는다.** 앱을 다시 열면 항상 오늘이다 — 과거 모드로 방치한 채 다음 운동을 오기록하는 사고를 막는 안전장치이므로 `store`에 넣지 말 것.
- 날짜에 의존하는 함수는 **`upsertEntry`·`lastPrev`·`activeEntry`·`renderDaily` 네 곳**이며 모두 `recDate`를 쓴다. `todayStr()`은 세션 요일 제안·`store.ui.date`·내보내기 파일명에만 남아 있다. **새 코드에서 "기록 대상 날짜"로 `todayStr()`을 부르면 버그다.**
- 진입점은 기록 화면 두 곳(상단 날짜 입력, 각 날짜 카드의 `＋ 이 날짜에 기록 추가`) → `openRecordDate(d)`. 복귀는 헤더 배너의 `오늘로` → `goToday()`. 메인 화면에는 과거 모드일 때만 배너가 뜬다.
- 과거 모드에서 억제되는 것: **진행 판정 화살표·ⓘ 팝업의 "진행 판정"**(판정은 최신 기록 기준이라 과거 화면에서 보이면 오독한다), **`store.sel` 쓰기**(그날 실제로 한 종목을 고르는 것이 오늘의 기본 종목을 덮지 않도록 `selTemp`로 격리 — `selFor()`로 읽는다), **`store.ui.session`/`date` 쓰기**(`setSession`이 분기한다).
- 세션 제안은 `sessionForDate(d)` — 그 날짜에 한쪽 세션 기록만 있으면 그 세션, 없으면 요일 기준.

## 렌더 파이프라인

전면 재렌더 방식이다. 부분 갱신 최적화를 넣지 말 것.

- `render()` = 배너(`renderDateBar()`) + 탭 상태 + `renderStrength()` + `renderScore()`. 종목 드롭다운 변경 시 `render()` 전체 호출.
- `renderBlock(block, labelPrefix)`가 카드 1개(드롭다운·ⓘ·축약 세트표기·판정 화살표·세트 체크박스·티어줄·무게/횟수/메모)를 만든다. 여기가 UI 변경의 중심.
- `refreshAfterRecordChange()` = `save()` + `renderRecords()` + `render()` + `renderDaily()` — 기록 화면에서 편집·삭제한 뒤 반드시 이걸 호출한다.
- **날짜별 집계는 `dayIndex()` 하나로 모은다** — 날짜 카드·달력·날짜 팝업이 공유한다(`{score, hasA, hasB, items[{order,name,key,sess,entry}], daily}`). 새로 집계 루프를 짜면 세 화면이 어긋난다. 세션 배지 클래스는 `sessBadge(v)`.
- 달력은 `renderCalendar()`(월 단위, `calCur`가 표시 중인 달). 기록 화면을 열 때 `calCur = null`로 이번 달부터 시작하고, 다음 달 버튼은 이번 달에서 비활성이다.
- 차트는 `lineChart()`가 손으로 그리는 SVG(340×140, 라이브러리 없음). 색은 CSS 변수(`var(--accent)`)를 그대로 SVG 속성에 넣어 테마를 따라간다.
- **오버레이는 `history.pushState` + `popstate`로 닫는다.** `#infoModal`·`#dayModal`·`#recordsView`를 열 때 state를 쌓고, `popstate` 한 곳에서 위 레이어부터(종목 정보 → 날짜 팝업 → 기록 화면) 닫는다. 닫기 버튼도 `history.back()`을 부른다(직접 `hidden=true` 금지 — 히스토리가 어긋난다). 새 오버레이를 추가하면 이 핸들러에 순서를 넣어야 한다.

## 진행 판정과 점수 (기계 규칙)

- `suggestFor(key, block)` — 정본 §7.1·7.3의 더블 프로그레션을 **마지막 세트 기록 기준**으로 판정. ①직전 횟수 ≥ `scheme` 상한 → ▲증량, ②증량 직후 횟수 < 하한 → ▼직전 무게 복귀, ③같은 무게 3세션 연속 반복 비증가 → ▼10% 감량. 결과는 카드의 화살표(title 툴팁)와 ⓘ 팝업 "진행 판정"에 동시 표시된다.
- 운동 점수 = 그날 각 종목의 `무게(kg 환산) × 마지막 세트 횟수` 합 (`toKg()`는 lb×0.45359237). 메인 카드는 현재 세션만 최근 12회, 기록 화면은 A/B 통합.

## Firebase 동기화

`index.html:1835`의 `FIREBASE_CONFIG`에 실제 config가 **이미 채워져 활성 상태**다(프로젝트 `workout-app-4efaa`). README와 `firebase-setup.md`의 "비활성(`null`)" 서술은 config 적용 이전 시점 기준이라 현재와 다르다.

- 문서 1개 모델: `users/{uid}` 에 `{payload: JSON.stringify(store), savedAt, email}`. 컬렉션 구조가 아니라 전체 기록을 통짜 문자열로 넣는다.
- 충돌 해결은 `savedAt` **last-write-wins**. `pullCloud()`는 원격 `savedAt`이 더 클 때만 로컬을 덮는다.
- 보안은 Firestore 규칙의 이메일 화이트리스트(`nisloke@gmail.com`)가 담당한다 — apiKey 공개는 정상. 사용자 추가는 콘솔 규칙 배열 수정이며 코드 변경이 아니다(`firebase-setup.md` 5단계).
- SDK는 `import()`로 동적 로드하므로 오프라인에서도 앱 본체는 동작해야 한다. 클라우드 실패는 상태줄 문구로만 알리고 로컬 저장은 계속된다 — 이 폴백을 깨지 말 것.

## 바꾸면 안 되는 설계 결정 (`PROMPT-HTML.md` §2·§3, 정정대장)

사용자가 명시적으로 뒤집기 전까지 유지한다. "더 나아 보여서" 되돌리지 말 것.

- **주차(week) 개념 금지.** 구성은 하나로 고정, 진행은 무게·횟수로만.
- **RIR 금지.** 문자열조차 등장하면 안 된다.
- **세트별 무게·횟수 입력칸 금지.** 종목당 무게 1·횟수 1·메모 1칸.
- **슈퍼세트는 세트 수가 같은 종목끼리만** 묶고 수행 방식을 화면에 설명한다.
- **모빌리티·워밍업·유산소는 초 단위 대본**이며 `rows[].sec` 합 = `totalSec`이어야 한다.
- **비대칭 장식 라벨 금지**("1순위" 등 전 순위를 못 붙일 표시).
- 용어는 헬스장 통용어(시티드 레그컬 O / 좌위 레그컬 X), 근육명은 정본 §1 분류 기준(대퇴사두·햄스트링, "쿼드"·"햄" 금지).
- 수행 큐는 ⓘ 팝업의 "수행 방법"에 있다(정정대장 #17로 카드 상시 노출에서 이동). 큐 문구는 이촌동/awesome 등 **전사 인용이 근거**이므로 임의 윤문 금지 — 출처는 `P1-종목후보표.md`·정정대장.

## 인코딩

`index.html`·`.md` 모두 **UTF-8 BOM**으로 저장한다(한글 깨짐 방지). Write/Edit 도구는 BOM을 떨어뜨리므로, `.md`는 PostToolUse 훅이 자동 부착하지만 **`.html`은 편집 후 BOM 유지 여부를 확인**하고 필요하면 재부착:

```powershell
[System.IO.File]::WriteAllText($p, [System.IO.File]::ReadAllText($p), [System.Text.UTF8Encoding]::new($true))
```
