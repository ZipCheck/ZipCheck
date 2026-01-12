# 🏙️ ZipCheck: 지도 기반 부동산 감정 스티커 커뮤니티

지도 기반 부동산 매물 정보와 사용자의 감정 표현을 결합한 커뮤니티 플랫폼

---

## 🧩 프로젝트 개요

### 문제 정의
부동산 정보는 넘쳐나지만, 특정 지역이나 매물에 대한 ‘분위기’나 ‘실거주 경험’ 같은 정성적인 정보는 얻기 어렵습니다. 텍스트 중심의 긴 후기는 읽기 부담스럽고, 별점만으로는 지역의 복합적인 평가를 파악하기 힘듭니다. 이는 정보 탐색의 피로도를 높이고, 확신 없는 결정을 내리게 하는 원인이 됩니다.

### 기존 방식의 한계
기존 부동산 앱들은 주로 매물의 정량적 정보(가격, 면적 등) 표시에 집중합니다. 커뮤니티 게시판이나 후기란이 있지만, 정보가 파편화되어 있고 지도와 직관적으로 연결되지 않아 원하는 정보를 찾기 번거롭습니다.

### 해결하고자 한 목표
ZipCheck는 지도 위에 표현되는 ‘감정 스티커’라는 직관적인 수단을 통해, 사용자가 특정 매물이나 지역에 대한 감정과 의견을 쉽고 재미있게 공유하고 탐색할 수 있는 환경을 제공하는 것을 목표로 합니다.

---

## 🎯 대상 사용자

- **주요 타겟**: 이사, 자취, 또는 실거주 목적으로 새로운 집을 탐색하는 20-40대 사용자
- **사용 시나리오**:
  1. 사용자는 지도를 통해 특정 지역의 매물을 확인하며, 주변에 부착된 스티커(예: 😊, 😠, ☕)를 통해 동네 분위기를 한눈에 파악합니다.
  2. 관심 있는 아파트 상세 페이지에서 AI가 요약한 거주민 리뷰와 실제 거래 데이터 차트를 확인하여 의사결정에 참고합니다.
  3. 커뮤니티 게시판에서 특정 부동산이나 지역에 대한 다른 사용자들의 심층적인 의견을 나누고 질문합니다.

---

## 💡 핵심 가치 (Value Proposition)

- **직관적인 정보 시각화**: 지도와 감정 스티커를 결합하여, 지역에 대한 긍정/부정적 감정 및 특징(예: 카페 많음, 야간 소음)을 즉각적으로 파악할 수 있습니다.
- **AI 기반 리포트**: 매물 상세 페이지에서 AI가 수많은 리뷰를 핵심만 요약한 리포트를 제공하여, 긴 글을 읽지 않아도 장단점을 빠르게 확인할 수 있습니다.
- **데이터 기반 의사결정**: 실거래가 데이터를 시각화한 차트를 제공하여, 사용자가 시세 변화를 쉽게 이해하고 합리적인 판단을 내릴 수 있도록 돕습니다.

---

## ⚙️ 주요 기능

- **사용자 / 인증**: JWT(Access/Refresh Token) 기반 회원가입, 로그인, 소셜 로그인 및 사용자 정보 관리
- **지도 / 매물**: Kakao Map API를 활용한 지도 기반 매물 조회, 아파트/오피스텔 필터링 및 상세 정보 페이지
- **스티커 / 감정 표현**: 지도 위 특정 위치에 감정 이모티コン과 짧은 코멘트를 부착하여 의견 공유
- **관심(찜) 기능**: 관심 있는 매물을 찜 목록에 추가하고 관리하는 기능
- **커뮤니티**: 자유롭게 글을 작성하고 소통할 수 있는 게시판 (게시글, 댓글, 좋아요/싫어요)
- **AI 기능**: `AiReport.vue` 컴포넌트를 통해 매물에 대한 사용자 리뷰를 요약하고 핵심 포인트를 제공

---

## 🛠 기술 스택

| 구분 | 기술 |
| --- | --- |
| Frontend | Vue.js 3, Pinia, Vue Router, Vite, Tailwind CSS, Chart.js |
| Backend | Spring Boot 3, Spring Security, MyBatis, JJWT |
| Database | MySQL 8.0 |
| Infra | AWS S3 (이미지 스토리지), Local Server |
| External API | Kakao Map API |

---

## 🧱 시스템 아키텍처

### 전체 구조
Vue.js로 빌드된 프론트엔드에서 사용자의 요청을 보내면, Spring Boot 기반의 백엔드 서버가 API 요청을 처리합니다. 데이터는 MySQL 데이터베이스에 영구 저장되며, MyBatis를 통해 SQL 쿼리를 관리합니다. 사용자 인증은 JWT 토큰 방식을 사용하며, 매물 관련 이미지 등 정적 파일은 AWS S3에 저장 및 서빙됩니다. 지도 표시는 Kakao Map API를 활용합니다.

![image](https://github.com/user-attachments/assets/c15b9e11-095d-4009-b634-1188448f2191)


### 주요 도메인
- **Users**: 사용자 정보, 인증, 권한
- **Deals**: 부동산 매물(아파트, 오피스텔) 정보 및 거래 내역
- **Stickers**: 사용자가 지도에 부착하는 감정 스D티커 및 코멘트
- **Interests**: 사용자의 관심(찜) 매물 목록
- **Boards**: 커뮤니티 게시글 및 댓글

### 인증 흐름
로그인 시 서버는 Access Token과 Refresh Token을 발급합니다. 클라이언트는 API 요청 시 Access Token을 헤더에 담아 보내며, 서버는 이를 검증하여 인가된 요청을 처리합니다. Access Token이 만료되면 Refresh Token을 사용하여 새로운 토큰 쌍을 재발급 받습니다.

---

## 🗄 데이터베이스 설계 요약

### 핵심 테이블
`users`, `houseinfo`(건물 정보), `housedeal`(거래 정보), `stickers`, `interests`, `board`, `comment` 등

### 설계 포인트
- **매물-스티커 관계**: `stickers` 테이블은 `houseinfo`의 PK(aptCode)를 FK로 참조하여 특정 매물과 스티커를 1:N 관계로 연결합니다.
- **관심 매물 조회**: `interests` 테이블을 `users`와 `houseinfo` 사이에 두어 다대다 관계를 구현하였고, 사용자가 찜한 매물 목록을 효율적으로 조회할 수 있도록 설계했습니다.
- **지역 코드**: 법정동 코드를 (`dongcodes`) 기준으로 `houseinfo`와 `housedeal`을 조인하여 지역별 매물 및 거래 정보를 빠르게 탐색할 수 있도록 구성했습니다.

---

## ▶️ 실행 방법 (Local)

### 요구 환경
- **Node.js**: 18.x 이상
- **JDK**: 17
- **MySQL**: 8.0

### 환경 변수
프로젝트 실행을 위해 frontend와 backend 디렉터리에 각각 `.env` 와 `application.properties` 파일을 생성하고 아래 내용을 채워야 합니다.

#### Frontend (`/zipcheck-frontend/vue-zipcheck/.env`)
```
VITE_KAKAO_MAP_KEY=your_kakao_map_javascript_key
VITE_API_BASE_URL=http://localhost:8080
```

#### Backend (`/zipcheck-backend/src/main/resources/application.properties`)
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/your_db?serverTimezone=UTC
spring.datasource.username=your_db_user
spring.datasource.password=your_db_password

jwt.secret=your_jwt_secret_key

cloud.aws.credentials.accessKey=your_s3_access_key
cloud.aws.credentials.secretKey=your_s3_secret_key
cloud.aws.s3.bucket=your_s3_bucket_name
cloud.aws.region.static=ap-northeast-2
```

### 실행 절차
1. **DB 스키마 및 초기 데이터 적재**: 제공되는 `schema.sql` 파일을 MySQL에서 실행하여 테이블을 생성합니다. (별도 파일 제공 가정)
2. **백엔드 서버 실행**: `zipcheck-backend` 프로젝트를 IDE에서 Spring Boot Application으로 실행하거나, Maven을 사용하여 빌드 후 실행합니다.
3. **프론트엔드 실행**: `zipcheck-frontend/vue-zipcheck` 디렉터리에서 아래 명령어를 실행합니다.
   ```bash
   npm install
   npm run dev
   ```
4. **접속**: 웹 브라우저에서 `http://localhost:5173` (Vite 기본 포트) 주소로 접속합니다.

---

## 📄 API 문서

- **API 문서 제공 방식**: 별도 문서는 제공하지 않으며, 백엔드 API 컨트롤러 코드를 통해 명세를 확인할 수 있습니다.
- **대표 API 예시**:
  - `GET /api/deals?dongCode=...&dealYear=...`: 특정 지역 및 연도의 매물 거래 정보 조회
  - `POST /api/stickers`: 지도에 새로운 스티커 추가
  - `GET /api/users/interests`: 현재 로그인된 사용자의 찜 목록 조회

---

## 🖼 화면 구성 / 데모

### 주요 화면
- **홈**: 서비스 소개 및 주요 기능 바로가기
- **지도**: 매물, 스티커를 탐색할 수 있는 메인 지도 페이지
- **매물 상세**: AI 리뷰 요약, 거래 내역 차트, 로드뷰 등을 포함한 상세 정보
- **스티커 페이지**: 특정 매물에 달린 스티커와 코멘트 모아보기
- **커뮤니티**: 게시글 목록 및 상세 보기
- **마이페이지**: 내 정보 수정, 내가 쓴 글/댓글, 찜 목록 확인

### 주요 화면 스크린샷

| 화면 1 | 화면 2 |
|---|---|
| <img src="zipcheck-frontend/vue-zipcheck/img/images.jpeg" width="400"/> | <img src="zipcheck-frontend/vue-zipcheck/img/images (1).jpeg" width="400"/> |
| **화면 3** | **화면 4** |
| <img src="zipcheck-frontend/vue-zipcheck/img/images (2).jpeg" width="400"/> | <img src="zipcheck-frontend/vue-zipcheck/img/images (3).jpeg" width="400"/> |
| **화면 5** | **화면 6** |
| <img src="zipcheck-frontend/vue-zipcheck/img/images (4).jpeg" width="400"/> | <img src="zipcheck-frontend/vue-zipcheck/img/images (6).jpeg" width="400"/> |

### 데모 링크
- https://drive.google.com/file/d/1nmo0_rQSROetxyF2gRXkRcC3DRU90sXx/view

---

## 👥 팀 구성 및 역할

- **팀 구성**: 개인 프로젝트
- **담당 역할**: Full-Stack (Frontend, Backend, Database 설계, AI 기능 연동, Infra)

---

## 📌 정리

이 프로젝트를 통해 Vue.js와 Spring Boot를 사용한 Full-Stack 웹 애플리케이션 개발 전 과정을 경험하고자 했습니다. 특히 외부 API(Kakao Map)와 AI 모델을 연동하고, 사용자의 인터랙션(스티커)을 핵심 기능으로 설계하며 문제 해결 능력과 기술적 성장 기회를 가질 수 있었습니다. 부동산 정보 비대칭 문제를 직관적이고 재미있는 방식으로 해결하는 경험을 통해 사용자 중심 서비스를 만드는 과정을 깊이 있게 학습했습니다.
