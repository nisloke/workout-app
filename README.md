# workout-app — 주 4회 2분할 운동 기록

개인용 운동 기록 웹앱(단일 `index.html`, 외부 라이브러리 없음). 모바일 기준.

- 라이브: https://nisloke.github.io/workout-app/
- 세션 A(밀기+하체 전면)/B(당기기+하체 후면), 종목 드롭다운 교체, 슬롯·종목별 무게(kg/lb)·횟수·메모 기록
- 운동 점수(무게 kg 환산×횟수 합) 차트, 체중·수면 기록, 날짜별 세부 기록 카드, 다크모드
- 저장: 현재 localStorage(기기 로컬) + JSON 내보내기/불러오기. Firebase 동기화 코드 내장(비활성) — 활성화 절차는 `firebase-setup.md`
- 프로그램 정본: 로컬 `workout-program/rework/프로그램-주4회-2분할.md` (앱 수치는 정본과 기계 대조로 일치 검증)
