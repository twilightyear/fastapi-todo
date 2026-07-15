# fastapi-todo 프로젝트

**Overview**

- **fastapi** 를 이용한 간단한 **TODO** 기능을 구현하는 개인 연습용 프로젝트이다.
- 전체적으로 다음과 같은 **dir** 구조를 지닌다.
```
fastapi-todo/
 ├── auth/
 │    └──  password.py
 ├── database/
 │    ├── db_connection.py
 │    └── orm.py
 ├── routers/
 │    ├── todo.py
 │    └── user.py
 ├── schema/
 │    ├── request.py
 │    └── response.py
 ├── model.py
 └── main.py
 ```

**Architecture**

<img width="798" height="510" alt="제목 없는 다이어그램 drawio (2)" src="https://github.com/user-attachments/assets/145993d3-b331-4bdf-a908-2761ee43975a" />

**Database**

<img width="435" height="151" alt="제목 없는 다이어그램 drawio (1)" src="https://github.com/user-attachments/assets/094f1111-1d1a-4cc6-a8fb-ae9533df5be7" />

**Tech Stack**
- **MySQL**
- **FastAPI**

**Key Highlights**
- 성능 개선 사례 : 아직 정리하지 않음
- 트러블슈팅 : 아직 정리하지 않음

**CI/CD**
- 프로젝트를 진행한 환경이 군대라는 특수한 공간이라 어쩔수 없이 커밋과 관련된 작업은 수동으로 해야만 했다. 물론 테스트와 같은 것들은 단편적으로만 확인이 가능했고 전반적인 테스트는 휴가 나가서 집에서 테스트를 해봐야했다. 바로 이전에 진행했던 **API** 프로젝트인 **community-backend-api** 프로젝트에서 **CI/CD** 파이프라인을 무조건 구축해보자 라는 의지가 있었다는 점을 고려하여 추후 프로젝트를 진행할때에는 **CI/CD** 파이프라인을 구축하여 생산적인 프로젝트 진행이 될 수 있도록 노력해보겠다.

**Goal**
- 본 프로젝트의 목표는 다음과 같다.
  1. **fastapi** 라이브러리 사용 친숙해지기
  2. **ERD** 설계 및 그리기 연습
  3. **Directory** / **System Structure** 등 과 같은 구조적 요소 설계 및 도출 연습

**Testing**
- 아직 정리하지 않음
