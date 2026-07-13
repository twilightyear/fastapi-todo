# fastapi-todo 프로젝트

**fastapi** 를 이용한 간단한 **TODO** 기능을 구현하는 개인 연습용 프로젝트입니다.

본 프로젝트의 목표는 다음과 같습니다.
  1. **fastapi** 라이브러리 사용 친숙해지기
  2. **ERD** 설계 및 그리기 연습
  3. **Directory** / **System Structure** 등 과 같은 구조적 요소 설계 및 도출 연습


# Entity Relationship Diagram (ERD)

---

<img width="435" height="151" alt="제목 없는 다이어그램 drawio (1)" src="https://github.com/user-attachments/assets/094f1111-1d1a-4cc6-a8fb-ae9533df5be7" />




# Directory Structure

---

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

# System Architecture

---

<img width="798" height="510" alt="제목 없는 다이어그램 drawio (2)" src="https://github.com/user-attachments/assets/145993d3-b331-4bdf-a908-2761ee43975a" />

