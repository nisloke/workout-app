# Firebase 연결 절차 (운동앱 클라우드 동기화)

작성: 2026-08-10. 앱에는 연결 코드가 이미 내장되어 있고(`FIREBASE_CONFIG = null` 상태로 비활성), 아래 절차 후 config만 채우면 활성화된다. PPL 일지와 같은 구조(Google 로그인 + 이메일 화이트리스트)다.

## 1단계 — 프로젝트 생성

1. https://console.firebase.google.com 접속 → **프로젝트 추가**
2. 프로젝트 이름: `workout-app` (아무거나 무방)
3. Google 애널리틱스: **사용 안 함** (불필요)
4. 요금제는 기본 **Spark(무료)** 그대로 — 이 앱 사용량(하루 수십 read/write)은 무료 한도의 1% 미만

> ⚠ Supabase 무료 2프로젝트 제한 같은 문제 없음 — Firebase는 프로젝트 수 제한이 넉넉하고, PPL 일지 프로젝트와 별개로 만들어도 된다.

## 2단계 — 웹 앱 등록 (config 얻기)

1. 프로젝트 개요 화면에서 **`</>` (웹)** 아이콘 클릭
2. 앱 닉네임: `workout-app` → **앱 등록** (Firebase Hosting 체크 불필요 — GitHub Pages 사용)
3. 표시되는 `firebaseConfig` 객체를 복사해 둔다:

```js
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "workout-app-xxxxx.firebaseapp.com",
  projectId: "workout-app-xxxxx",
  storageBucket: "workout-app-xxxxx.appspot.com",
  messagingSenderId: "...",
  appId: "..."
};
```

## 3단계 — Google 로그인 활성화

1. 왼쪽 메뉴 **빌드 → Authentication** → **시작하기**
2. **Sign-in method** 탭 → **Google** → **사용 설정** → 지원 이메일 선택 → 저장
3. **Settings → 승인된 도메인** 탭에서 **`nisloke.github.io` 추가** (localhost·firebaseapp.com은 기본 포함)

## 4단계 — Firestore DB 생성

1. 왼쪽 메뉴 **빌드 → Firestore Database** → **데이터베이스 만들기**
2. 위치: **asia-northeast3 (서울)**
3. 모드: **프로덕션 모드로 시작** (규칙은 다음 단계에서 교체)

## 5단계 — 보안 규칙 붙여넣기 (그대로 입력)

**규칙** 탭 → 전체를 아래로 교체 → **게시**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{uid} {
      allow read, write: if request.auth != null
        && request.auth.uid == uid
        && request.auth.token.email in ['nisloke@gmail.com'];
    }
  }
}
```

- 효과: **본인 Google 계정으로 로그인한 본인 문서만** 읽기/쓰기 가능. 화이트리스트에 없는 계정은 로그인해도 거부된다.
- 다른 사람(예: 가족)을 추가하려면 배열에 이메일을 추가: `in ['nisloke@gmail.com', 'xxx@gmail.com']`
- 컬렉션·문서를 미리 만들 필요 없음 — 앱이 첫 동기화 때 `users/{uid}` 문서를 자동 생성한다.

## 6단계 — 앱에 config 적용

`index.html`에서 `var FIREBASE_CONFIG = null;` 을 찾아 2단계의 객체로 교체:

```js
var FIREBASE_CONFIG = {
  apiKey: "AIza...",
  authDomain: "workout-app-xxxxx.firebaseapp.com",
  projectId: "workout-app-xxxxx",
  storageBucket: "workout-app-xxxxx.appspot.com",
  messagingSenderId: "...",
  appId: "..."
};
```

커밋·푸시하면 끝. **config를 나(Claude)에게 붙여넣어 주면 교체·푸시까지 대신 처리한다.** (apiKey는 웹 앱 특성상 공개되어도 되는 값 — 보안은 5단계 규칙+화이트리스트가 담당)

## 동작 방식 (내장 코드)

- config가 null이면: 지금처럼 localStorage만 사용, 외부 요청 0.
- config 적용 후: "기록 데이터 관리" 카드에 **Google 로그인** 버튼과 상태줄이 나타난다.
- 로그인하면: 원격(`users/{uid}` 문서 1개, `payload`=전체 기록 JSON)과 로컬 중 **savedAt이 최신인 쪽이 이긴다**(기기 간 last-write-wins). 이후 입력할 때마다 1.5초 디바운스로 자동 푸시.
- 오프라인/실패 시: 로컬 저장은 항상 유지되고 상태줄에 "동기화 실패(로컬에는 저장됨)" 표시.
- 용량: 기록 1년치 ≈ 수백 KB — Firestore 문서 한도(1MB) 내. 수년 누적 시 연 단위 문서 분할로 전환 예정.
