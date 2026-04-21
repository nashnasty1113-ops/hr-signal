# HR 시그널 — 설정 가이드

## 파일 구조
```
hr-signal/
├── index.html        ← 메인 뉴스레터 페이지
├── admin.html        ← 관리자 편집 페이지
├── sample-data.json  ← Firebase 초기 데이터
└── README.md
```

## 1단계 — Firebase 설정

1. https://console.firebase.google.com 접속
2. 새 프로젝트 생성 (예: `hr-signal`)
3. Realtime Database → 데이터베이스 만들기 → 테스트 모드
4. 프로젝트 설정 → 웹 앱 추가 → Firebase 설정값 복사

## 2단계 — Firebase 설정값 입력

`index.html` 과 `admin.html` 두 파일에서 아래 부분을 찾아 교체:

```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",           // ← 실제 값으로 교체
  authDomain: "YOUR_PROJECT...",
  databaseURL: "https://YOUR_PROJECT-default-rtdb...",
  ...
};
```

## 3단계 — 비밀번호 변경

`index.html` 에서:
```javascript
const ADMIN_PASSWORD = 'maeil2024';  // ← 원하는 비밀번호로 변경
```

## 4단계 — 초기 데이터 입력

Firebase 콘솔 → Realtime Database → 데이터 탭 → 오른쪽 점 메뉴 → JSON 가져오기
→ `sample-data.json` 파일 업로드

## 5단계 — Vercel 배포

```bash
# Vercel CLI 설치 (최초 1회)
npm i -g vercel

# hr-signal 폴더에서
vercel

# 이후 수정 시
vercel --prod
```

## 기사 업데이트 방법

### Claude에게 요청하는 경우
1. 키워드 엑셀 파일 공유
2. "HR 시그널 #13 기사 찾아줘" 요청
3. Claude가 JSON 형태로 반환
4. 관리자 페이지에서 붙여넣기

### 직접 입력하는 경우
1. `your-url.vercel.app/admin.html` 접속
2. 비밀번호 입력
3. `+ 새 에디션 추가` 클릭
4. 기사 추가 → 저장 & 발행

## 관리자 URL
```
https://your-project.vercel.app/admin.html
```
(index.html 우상단 "관리자 ↗" 클릭 → 비밀번호 입력해도 이동)
