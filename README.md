[README.md](https://github.com/user-attachments/files/28243663/README.md)
# HR시그널 (HR Signal)

> 매일유업 피플팀 주간 HR 이슈 뉴스레터 시스템

Firebase Realtime Database 기반의 콘텐츠 관리 시스템(CMS)으로,  
별도 서버 없이 정적 HTML 파일만으로 운영되는 사내 HR 뉴스레터 플랫폼입니다.

---

## 📌 개요

| 항목 | 내용 |
|------|------|
| 운영 주체 | 매일유업 피플팀 |
| 서비스 형태 | 정적 웹사이트 (Firebase Hosting) |
| 데이터베이스 | Firebase Realtime Database |
| 주요 기능 | 주간 HR 뉴스레터 발행 / 기사 관리 / 아카이브 검색 / 조회수 분석 |

---

## 🗂 파일 구조

```
/
├── index.html        # 독자용 뉴스레터 페이지
├── admin.html        # 관리자용 CMS 페이지
└── firebase.json     # Firebase Hosting 배포 설정
```

### index.html — 독자 페이지
- 이번 주 호: Top5, Editor's Pick, 카테고리별 기사 목록
- 지난 호 아카이브: 전체 기사 검색 (제목 / 카테고리 / 키워드)
- 기사 상세 모달: 요약 · 핵심포인트 · 원문링크
- 반응형 디자인 (모바일 지원)

### admin.html — 관리자 페이지
- 에디션 생성 · 삭제 (사이드바)
- 기사 CRUD (카테고리 · 태그 · 핵심포인트 · URL 관리)
- Top5 & Editor's Pick 슬롯 드래그&드롭 지정
- 실시간 저장 & 발행 (Firebase 직접 저장 → 독자 페이지 즉시 반영)
- JSON 뷰어 / 편집기 (데이터 직접 확인 및 수정)
- 조회수 분석 대시보드

---

## 🛠 기술 스택

| 구분 | 기술 |
|------|------|
| Frontend | HTML / CSS / JavaScript (Vanilla, 외부 프레임워크 없음) |
| Database | Firebase Realtime Database |
| Hosting | Firebase Hosting |
| 폰트 | Google Fonts (Noto Serif KR, Noto Sans KR, Playfair Display) |

---

## 🗄 Firebase DB 구조

```
editions/
  {editionKey}/               ← 에디션 키 (YYYY-MM-DD_난수)
    title: string             ← 에디션 제목 (예: "HR시그널 #12")
    date: string              ← 발행일 (YYYY-MM-DD)
    period_start: string      ← 기간 시작일
    period_end: string        ← 기간 종료일
    period: string            ← 표시용 기간 문자열 (자동 생성)
    intro: string             ← Editor's Note 소개글
    top5: array               ← 편집자 선정 Top5 기사 (최대 5개)
      [].title: string
      []._id: string          ← 기사 고유 ID
      [].category: string
      [].date: string
      [].source: string
      [].summary: string
      [].points: array        ← 핵심 포인트 목록
      [].tags: array
      [].url: string
      [].badge: object|null   ← { label: "HOT", type: "hot" }
    editors_pick: object|null ← Editor's Pick 기사 1건
    sections: array           ← 카테고리별 섹션
      [].title: string        ← 카테고리명
      [].articles: array      ← 기사 목록

views/
  {editionKey}/
    total: number             ← 에디션 총 페이지뷰
    articles: object          ← 기사별 클릭수 { _id: count }
    lastUpdated: number       ← timestamp
```

---

## 🚀 배포 방법

### 콘텐츠 업데이트 (코드 배포 없이)

```
1. 브라우저에서 [배포 도메인]/admin.html 접속
2. 관리자 비밀번호 입력
3. [+ 새 에디션 추가] → 기사 입력 → [💾 저장 & 발행]
   → index.html에 즉시 반영 (별도 배포 불필요)
```

### 코드 수정 후 재배포

```bash
# Firebase CLI 설치 (최초 1회)
npm install -g firebase-tools
firebase login

# 배포
firebase deploy --only hosting
```

---

## 📋 주간 운영 프로세스

| 단계 | 작업 | 소요 시간 |
|------|------|-----------|
| STEP 1 | 주간 HR 기사 수집 | 2~3시간 |
| STEP 2 | admin.html 접속 | 1분 |
| STEP 3 | 새 에디션 생성 (제목·날짜·소개글 입력) | 5분 |
| STEP 4 | 기사 입력 (카테고리·요약·포인트·URL) | 30~60분 |
| STEP 5 | Top5 슬롯 지정 및 뱃지 설정 | 5분 |
| STEP 6 | Editor's Pick 지정 | 2분 |
| STEP 7 | 저장 & 발행 | 1분 |
| STEP 8 | index.html에서 최종 검토 | 10분 |

---

## 📂 카테고리 목록

`노사관계` `노동법/정책` `채용/퇴직` `조직/문화` `보상/복리후생` `교육/개발` `AI/디지털HR` `글로벌HR` `기타`

---

## ⚠️ 보안 주의사항

- 관리자 비밀번호가 `index.html` 소스에 평문 저장되어 있음 → **운영 환경에서는 변경 필요**
- Firebase API Key가 소스코드에 노출되어 있음 → **Firebase Console에서 도메인 제한 설정 권장**
- Firebase 무료 플랜(Spark) 한도: 10GB/월 전송량 초과 시 유료 전환 필요

---

## 📝 업데이트 이력

| 날짜 | 내용 | 담당 |
|------|------|------|
| 2026-05 | 초기 버전 개발 완료 | 김창중 |
| | Firebase Realtime DB 기반 CMS 구현 | |
| | Top5 드래그&드롭, Editor's Pick, JSON 편집기 | |
| | 아카이브 탭, 조회수 분석 대시보드 | |

---

© 2026 매일유업 피플팀
