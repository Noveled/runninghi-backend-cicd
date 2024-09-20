# Running Hi Backend
<div align="center">
<h2>[2024] RunningHi - Backend 🎮</h2>
</div>

RunningHi는 리액트 기반 웹 서비스로, 사용자가 지도 위에 마커를 등록하여 러닝 코스를 만들고 이를 다른 사용자와 공유할 수 있는 기능을 제공합니다. 이 프로젝트는 코드랩 AICC 3기 1차 팀 프로젝트로 진행되었으며, [Runnect](https://github.com/Runnect/Runnect-Android) 프로젝트를 참고하여 제작되었습니다. <br>
RunningHi 의 Frontend 가 궁금하다면?
- Frontend Repository: [RunningHi Frontend](https://github.com/Noveled/runninghi-frontend-cicd)  

## 목차
- [프로젝트 개요](#프로젝트-개요)
- [기능 설명](#기능-설명)
- [주요 엔드포인트](#주요-엔드포인트)
- [CI/CD 플로우 차트](#ci-cd-플로우-차트)
- [오류 보고](#오류-보고)
- [레퍼런스](#레퍼런스)

---

## 프로젝트 개요
- **프로젝트 이름**: RunningHi - Backend
- **프로젝트 기간**: 2024.08.20 - 2024.09.20
- **사용 기술**: `Node.js`, `PostgreSQL`
- **팀 구성**: 김민식(팀장), 강민주, 손주현
<table>
  <tr>
    <!-- first -->
    <td align="center">
      <a href="https://github.com/Noveled">
        <img src="https://github.com/Noveled.png" width="100px;" alt="김민식"/><br />
        <sub><b>김민식</b></sub>
      </a>
    </td>
    <!-- second -->
    <td align="center">
      <a href="https://github.com/Noveled">
        <img src="https://github.com/Noveled.png" width="100px;" alt="강민주"/><br />
        <sub><b>강민주</b></sub>
      </a>
    </td>
    <!-- third -->
    <td align="center">
      <a href="https://github.com/Noveled">
        <img src="https://github.com/Noveled.png" width="100px;" alt="손주현"/><br />
        <sub><b>손주현</b></sub>
      </a>
    </td>
  </tr>
</table>
- **Demo Link**: [RunningHi Backend](https://runninghibackend.siinat.com/)
- **Architecture**:
![백엔드 아키텍처](https://github.com/Noveled/runninghi-backend-cicd/blob/main/docs/images/backend_architecture.PNG)
위 그림은 백엔드의 전체적인 구조를 보여주며, 배포와 CI/CD 기능까지 포함된 BackEnd 아키텍처를 나타냅니다.

---

## 기능 설명
RunningHi Backend는 다음과 같은 주요 기능들을 API로 제공합니다:

|![코스 정보 요청 화면](https://github.com/Noveled/runninghi-backend-cicd/blob/main/docs/images/backend_get_course.PNG)|![편의시설 정보 요청 화면](https://github.com/Noveled/runninghi-backend-cicd/blob/main/docs/images/backend_get_facilities.PNG)|
|:---:|:---:|
|**코스 정보 요청 화면**|**편의시설 정보 요청 화면**|

1. **코스 정보 요청**   
   - 코스명, 총거리, 등록자 정보 등 기본적인 CRUD 기능을 지원합니다.

2. **유저 정보 요청** 
   - 사용자 아이디, 비밀번호, 프로필 사진 등 기본적인 유저 정보를 다룹니다.

3. **편의시설 정보 요청** 
   - 시설별 위치 정보 및 시설 타입 조회를 제공합니다.

4. **이미지 관리**  
   - 유저 프로필 및 코스 설명 사진 관리를 효율적으로 처리합니다.

5. **조회수 관리**  
   - 명확한 조회수 관리를 위한 기능으로, 향후 좋아요 기능 등에 응용할 수 있습니다.

---

## 주요 엔드포인트
> **자세한 API 명세는 DOCS 폴더의 PPT에서 확인할 수 있습니다.**  
> 여기서는 주요 엔드포인트를 요약합니다.

### POST /course (코스 등록)
새로운 코스를 등록하는 요청을 처리합니다. 사용자가 코스를 생성할 때 호출되는 API의 로직입니다.
- **HTTP 메서드**: `POST`
- **엔드포인트**: `/course`
- **요청 본문**: JSON 형식으로 코스 정보를 포함해야 합니다.

#### 요청 예시:
```json
{
  "course_name": "City Run",
  "user_id": 123,
  "content": "A great running route through the city.",
  "distance": 5.0,
  "waypoint": [{"lat": 37.123, "lng": 127.456}],
  "city": "Seoul",
  "is_private": false,
  "url": "https://example.com/course_image.jpg",
  "center": {"lat": 37.123, "lng": 127.456},
  "level": "beginner"
}
```

#### 요청 파라미터:
- `course_name` (필수): 코스 이름
- `user_id` (필수): 코스 등록 유저 ID
- `content` (필수): 코스 설명
- `distance` (필수): 총 거리
- `waypoint` (필수): 경유지 목록 (JSON 배열)
- `city` (필수): 도시 이름
- `is_private` (필수): 코스 공개 여부 (`true` or `false`)
- `url` (필수): 코스 이미지 URL
- `center` (필수): 코스 중심 좌표
- `level` (필수): 난이도 (초급, 중급 등)

#### 응답 예시:
- **성공 시**: `201 Created`
  ```json
  {
    "message": "Course Created Successfully"
  }
  ```

- **오류 시**: `500 Internal Server Error`
  ```json
  {
    "error": "Error message"
  }
  ```

- **GET /course** (코스 조회)  
  코스 정보를 조회하는 API의 로직을 담당합니다. 사용자가 특정 코스를 조회하거나 모든 코스를 요청할 때 호출됩니다.
  

### GET /course (코스 정보 조회)

- **HTTP 메서드**: `GET`
- **엔드포인트**: `/course`
- **쿼리 파라미터** (선택 사항):
  - `userId`: 특정 사용자가 등록한 코스를 조회할 때 사용
  - `isMarathon`: 마라톤 코스 여부에 따라 필터링
  - `isVisible`: 검색 가능 여부에 따라 필터링 (삭제되지 않은 코스만 조회)

#### 요청 예시:
```
GET /course?userId=123&isMarathon=true&isVisible=true
```

#### 응답 예시:
- **성공 시**: `200 OK`
  ```json
  [
    {
      "course_id": 1,
      "user_id": 123,
      "course_name": "City Marathon",
      "total_distance": 42.195,
      "is_marathon": true,
      "is_visible": true,
      "thumbnail_id": "https://example.com/thumbnail.jpg"
    },
  ]
  ```

- **오류 시**: `500 Internal Server Error`
  ```json
  {
    "error": "Error message"
  }
  ```

- **GET /facilities** (편의시설 정보 조회)  
  주변 편의시설 정보를 가져오는 API의 로직입니다. 시설별 위치와 타입 정보를 반환하는 기능을 합니다.
  예시 생략

- **GET /user** (유저 정보 조회)  
  특정 유저의 정보를 조회하는 API를 처리합니다. 사용자 프로필이나 계정 정보를 요청할 때 사용됩니다.
  예시 생략

---

## CI/CD 플로우 차트
코스 등록, 수정, 삭제의 CI/CD 플로우는 다음과 같습니다:

**Flowchart 이미지**  
![CICD01](https://github.com/Noveled/runninghi-backend-cicd/blob/main/docs/images/rinninghi_cicd01.PNG)
![CICD02](https://github.com/Noveled/runninghi-backend-cicd/blob/main/docs/images/rinninghi_cicd02.PNG)
**Flowchart 설명**  

자세한 CI/CD 과정은 제 블로그인 [신나 티스토리 블로그](#)에서 확인할 수 있습니다.

---

## 오류 보고

### 해결 완료
- **CORS 에러**: `cors` 모듈을 활용하여 허용할 페이지와 `credentials` 여부를 설정하였습니다. cors모듈 활용시 줄바꿈 및 주석을 포함하는 경우 정상적으로 동작하지 않았는데, 줄바꿈은 유지하되 주석을 제거하여 해결되었습니다.

### 해결 미완료
- **코스 상세보기 페이지**: 현재 전체 데이터를 조회 후 `props`로 넘겨주는 방식에서 몇 가지 문제 발생(예: 코스 공유, 페이지 바로 접근). 이를 엔드포인트로 분리하면 해결될 것으로 예상됩니다.

---

## 레퍼런스
- **Runnect**: [링크](https://github.com/Runnect/Runnect-Android)
- **참고 블로그**: [링크](#)
