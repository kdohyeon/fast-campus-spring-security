# 넷플릭스 멤버십 구독 서비스

## 기능 정의

### 사용자, 유저, 회원, 고객
- 회원가입
- 로그인
- 로그아웃
- 탈퇴

### 멤버십
- 멤버십 가입
- 멤버십 탈퇴
- 멤버십 변경
- 멤버십에 따른 차별화 기능 제공

### 넷플릭스 관련 기능
- 동영상 시청
    - [https://developer.themoviedb.org/reference/discover-movie](https://developer.themoviedb.org/reference/discover-movie)
- 컨텐츠 다운로드
- 디바이스
- 광고
- 프로필

### 어드민
- 멤버십 CRUD
- 회원 RUD
  - 회원 block 등

## 사용자 유형 정의

멤버십 구독 서비스에는 여러 유형의 사용자가 존재한다.

- 게스트 (Guest)
  - 로그인을 하지 않은 사용자
  - 인증이 되지 않은 사용자
- 유저 (User)
  - 일반 회원가입, 소셜 로그인 등 인증 과정을 통해 로그인에 성공한 사용자
- 고객 (Customer)
  - 유저이면서 멤버십 상품을 구독하는 사용자

## 멤버십 유형 정의
- `STANDARD_WITH_AD` : 광고형 스탠다드
- `STANDARD_WITHOUT_AD` : 일반 스탠다드 (광고X)
- `PREMIUM` : 프리미엄

## 멤버십 별 혜택
|              | 게스트 | 유저  | 광고형 스탠다드 | 일반 스탠다드 | 프리미엄 |
|--------------|-----|-----|----------|---------|------|
| 홈 화면 접근      | O   | O   | O        | O       | O    |
| 영상 시청 페이지 접근 | X   | O   | O        | O       | O    |
| 프로필          | X   | 1   | 2        | 2       | 4    |
| 컨텐츠 다운로드     | X   | X   | 10       | 20      | 30   |
| 광고 노출        | X   | X   | O        | X       | X    |
| 동시 디바이스 연결   | X   | X   | 1        | 2       | 3    |
| 프리미엄 영상 시청   | X   | X   | X        | X       | O    |

## 화면

- 홈 화면
- 로그인 화면
- 회원가입 화면
- 영상 리스트 화면
- 영상 상세 조회 화면
- 내 정보 화면

## 사용자 시나리오

각 상황별 사용자의 시나리오를 정리한다.

### 로그인, 로그아웃, 회원가입

- 회원가입을 하지 않은 사용자
    - 회원가입 페이지로 이동하여 신규 유저로 회원가입을 할 수 있다.
    - 소셜 로그인으로 회원가입을 할 수 있다.
- 회원가입을 한 사용자
    - ID, PW 로 로그인을 할 수 있다.
    - 소셜 로그인으로 로그인을 할 수 있다.
- 로그인에 성공하면 영상 리스트 화면으로 랜딩한다.
- 회원가입에 성공하면 해당 정보를 기반으로 로그인을 하며 영상 리스트 화면으로 랜딩한다.
- 로그아웃에 성공하면 홈화면으로 랜딩한다.

### 내 정보
- 내 정보를 수정할 수 있다.
- 비밀번호를 변경할 수 있다.
- 구독하고 있는 멤버십을 수정할 수 있다.
- 회원 탈퇴를 할 수 있다.

### 어드민

#### 사용자
- 사용자를 조회할 수 있다. 페이지네이션 기능이 제공되어야 한다.
- 사용자를 block 처리할 수 있다.

#### 멤버십
- 멤버십을 신규로 등록할 수 있다.
- 기존 멤버십을 수정할 수 있다.
- 기존 멤버십을 삭제할 수 있다. 이미 해당 멤버십으로 가입이 되어 있는 경우에는 삭제가 불가능하다.
- 멤버십을 조회할 수 있다.

## 사용자 상태
- ACTIVE: 정상
- BLOCKED: 제한
- INACTIVE: 탈퇴

## 사용자 생명 주기
- 회원가입에 성공하면 ACTIVE
- 어드민에 의해 제한이 되면 BLOCKED
    - 다시 ACTIVE 로 돌아갈 수 있음
- 회원 탈퇴를 하면 INACTIVE

## API 명세

### 사용자, 회원

- 로그인 - POST /api/v1/user/{userId}/login
- 로그아웃 - POST /api/v1/user/{userId}/logout
- 회원가입 - POST /api/v1/user/{userId}/signup
- 이메일 중복 조회 - POST /api/v1/user/{userId}/email/duplicate
- 닉네임 중복 조회 - POST /api/v1/user/{userId}/nickname/duplicate
- 회원정보 조회 - GET /api/v1/user/{userId}
- 회원정보 수정 - PUT /api/v1/user/{userId}
- 비밀번호 조회 - GET /api/v1/user/{userId}/password
- 비밀번호 변경 - PUT /api/v1/user/{userId}/password
- 회원 탈퇴 - DEL /api/v1/user/{userId}

### 인증
- 토큰 생성 - POST /api/v1/auth/access-token/reissue

### 프로필

- 프로필 목록 조회 - GET /api/v1/user/{userId}/profile
- 프로필 상세 조회 - GET /api/v1/user/{userId}/profile/{profileId}
- 프로필 수정 - PUT /api/v1/user/{userId}/profile/{profileId}

### 상품 (영화)

- 영화 목록 조회 - GET /api/v1/movie/list
- 영화 상세 조회 - GET /api/v1/movie/{movieId}/detail
- 컨텐츠 다운로드 - POST /api/v1/movie/{movieId}/download

### 어드민

- 사용자 제한 처리 - POST /api/v1/admin/user/{userId}/block
- 사용자 목록 조회 - POST /api/v1/admin/user/search
- 멤버십 신규 등록 - POST /api/v1/admin/membership/register
- 기존 멤버십 수정 - PUT /api/v1/admin/membership/{membershipId}
- 멤버십 삭제 - DEL /api/v1/admin/membership/{membershipId}
- 멤버십 조회 - GET /api/v1/admin/membership/{membershipId}