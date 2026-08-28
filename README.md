## **Overview**

-   **본 프로젝트는 Backend** 관련 경험을 쌓아보고자 간단하게 진행해본 간단한 **연습용 개인 프로젝트** 이다.

> [https://github.com/twilightyear/express-community-board/tree/main](https://github.com/twilightyear/express-community-board/tree/main)

## **Database ERD**

![제목 없는 다이어그램 drawio](https://github.com/user-attachments/assets/03b7b78f-64f4-47c0-b27b-5a3c489790be)

## **System Architecture**

![제목 없는 다이어그램 drawio (2)](https://github.com/user-attachments/assets/84d0de95-3f2d-498b-b979-b5c28355416a)

## **Tech Stack**

-   **_NodeJS :_** 브라우저 환경에서 벗어나서 **Javascript** 를 실행할 수 있는 런타임 환경 제공
-   **_Express :_ Node.js** 환경에서 **Rest API** 와 웹서버를 효율적으로 구축하기 위한 프레임워크
-   **_MySQL :_** 관계형 데이터베이스 **(RDBMS)**
-   **_Sequelize :_ Node.js** 용 **ORM**

## **프로젝트 목표**

-   **NodeJS, Express, MySQL, Sequelize** 를 활용하여 간단한 **API** 를 직접 만들어보며 친숙해지고싶다.
-   **MySQL Workbench** 등을 활용한 프로젝트 진행을 통하여 실전 활용 능력을 기르고 싶다.
-   **Post Man** 을 활용하여 **API** 테스트 경험을 해보고싶다.
-   위에서 서술한 여러 능력을 토대로 기본적인 **API** 구축 관련 지식을 쌓아 향후 더 크고 의미있는 프로젝트에서 활용해보고자 한다.

## **트러블슈팅**

-   관계형 데이터베이스로 정상적으로 만들어졌다면 **CommunityId** 값이 자동으로 생성되어야하지만  
    생성되지 않았을 경우에는 직접 추가를 해줘야 한다.
-   먼저 아래 명령어로 데이터베이스에 접근하여 쿼리문을 실행할 수 있는 환경을 만들어줘야 했다.

```
mysql -u [ User ID ] -p [ Database Name ]
```

-   위 명령어를 실행하고 **mysql** 패스워드를 입력하면 터미널 기반 쿼리문 제어창에 접근할 수 있었다.  
    이후 아래 명령어로 누락되었던 **Foreign Key** 인 **CommunityId** 의 칼럼을 수동으로 생성했다.

```
ALTER  TABLE Comments ADD  COLUMN CommunityId INT, ADD  CONSTRAINT fk_CommunityId FOREIGN KEY (CommunityId) REFERENCES Communities(id) ON  DELETE CASCADE;
```

-   또한 createdAt 칼럼과 **updatedAt** 칼럼의 **Default / Expression** 영역에 **_CURRENT\_TIMESTAMP_** 와 **CURRENT\_TIMESTAMP ON UPDATE CURRENT\_TIMESTAMP** 적절하게  
    설정을 해줘야 했다.
-   이렇게 해결할 수 있었지만 근본적인 이유를 추적해본 결과, **model** 파일 중에서 **comment.js** **community.js** 파일 내의 대소문자 불일치로 인한 문제 발생이라는 점을 찿을 수 있었다.

```
//comment.js
'use strict';
const {
  Model
} = require('sequelize');
module.exports = (sequelize, DataTypes) => {
  class comment extends Model {
    /**
     * Helper method for defining associations.
     * This method is not a part of Sequelize lifecycle.
     * The `models/index` file will call this method automatically.
     */
    static associate(models) {
      comment.belongsTo(models.community, {
        foreignKey:  'CommunityId',
        onDelete:  'CASCADE'
      });
    }
  }
  comment.init({
    text: DataTypes.STRING
  }, {
    sequelize,
    modelName: 'comment',
  });
  return comment;
};
```

```
//community.js
'use strict';
const {
  Model
} = require('sequelize');
module.exports = (sequelize, DataTypes) => {
  class community extends Model {
    /**
     * Helper method for defining associations.
     * This method is not a part of Sequelize lifecycle.
     * The `models/index` file will call this method automatically.
     */
    static associate(models) {
      community.hasMany(models.comment, {
        foreignKey:  'CommunityId',
        onDelete:  'CASCADE'
      });
    }
  }
  community.init({
    title: DataTypes.STRING,
    content: DataTypes.STRING
  }, {
    sequelize,
    modelName: 'community',
  });
  return community;
};
```

-   **static associate(models)** 부분, 즉 모델을 **migrate** 하고 자동 생성된 파일을 수정하는 과정에서 생긴 오류였다.
-   데이터베이스를 다시 재설정하고 파일 내 서로의 연관성을 정립하는 부분의 대소문자의 오타를 수정해주고 다시 구동을 시도 한 결과 자동으로 **CommunityID** 값이 생성되지 않는 문제를 해결할 수 있었다.

## **CI/CD**

-   프로젝트 작업을 진행하며 브랜치 생성과 반복되는 수동 커밋을 통하여 깃허브라는 형상관리 툴의 기능은 다양하게 이용 및 실험 해봤지만 결국 반복되는 작업의 비효율성을 느꼈다.
-   이를 개선하기 위하여 추후 프로젝트에서는 **Github Actions** 를 활용한 **CI/CD** 파이프라인을 구축하여 더욱 효율적이고 생산적인 프로젝트 진행이 될 수 있도록 하는 것이 목표이다.

## **Testing**

-   먼저 초기화를 해준다.

```
sequelize init
```

```
sudo npm cache clean -f
```

```
npx sequelize-cli init
```

-   이후 데이터베이스를 생성한다.

```
npx sequelize-cli db:create
```

-   모델 파일을 생성한다.

```
sequelize model:generate --name community --attributes title:string,content:string
sequelize model:generate --name comment --attributes text:string
```

```
npx sequelize-cli db:migrate
```

-   **/models/comment.js** 에 다음과 같은 코드를 삽입하여 관계형 데이터베이스를 정립한다.

```
static  associate(models) {
    comment.belongsTo(models.community, {
        foreignKey:  'CommunityId',
        onDelete:  'CASCADE'
    });
}
```

-   **/models/community.js** 에도 다음과 같은 코드를 삽입하여 관계형 데이터베이스를 정립한다.

```
static  associate(models) {
    community.hasMany(models.comment, {
        foreignKey:  'CommunityId',
        onDelete:  'CASCADE'
    });
}
```

-   위 트러블 슈팅 과정에서 설명했지만, 위 과정에서 오타 때문에 상당한 문제가 발생했었다.
-   **MySQLWorkbench** 를 이용하여 데이터베이스를 설정해야 했다.
-   **MySQLWorkbench** 의 메인화면은 다음과 같다.

![image](https://github.com/user-attachments/assets/3dfa581f-876f-4e3d-9dca-0f5cb8b009a2)

-   아래의 명령어를 실행하면 서버가 시작된다.

```
node app.js
```

-   성공적으로 실행되었다면 다음과 같은 결과가 나온다.

![image](https://github.com/user-attachments/assets/a8e2b32a-c3fa-4033-a3d6-12f6ae09b895)

-   이제 본격적으로 **POSTMAN** 을 사용하여 9가지의 구현한 기능에 대하여 테스트를 해보자.

![image](https://github.com/user-attachments/assets/ad2df21b-ff17-4273-a761-198aaa5c181d)

-   게시글 생성을 해보자.
-   게시글 생성 기능은 **/createCommunity** 를 통하여 생성을 진행한다.

![image](https://github.com/user-attachments/assets/fda9e209-3003-4cc2-a90d-704f853bbe60)

-   **req** 를 통하여 **title** 과 **content** 칼럼에 대응될 값들을 **POST**를 통하여 받는다.
-   **id** 값은 처리될때마다 자동으로 순차적으로 생성되며, **createdAt** 과 **updatedAt** 또한 위에서 설정한 **CURRENT\_TIMESTAMP** 와 **CURRENT\_TIMESTAMP ON UPDATE CURRENT\_TIMESTAMP** 덕분에 자동으로 처리된다.
-   게시글 전체 조회를 해보자. 전체 조회 이전에 게시글 생성 및 삭제과정을 임의로 몇번 진행하였다.

![image](https://github.com/user-attachments/assets/d814ec16-c7c6-4d3c-b501-b33ce32bdb09)

-   게시글 전체 조회 기능은 **/getCommunity** 를 통하여 조회를 진행한다.
-   게시글 상세조회를 해보자. **게시글 ID** 를 지정하여 단일 게시글만 불러올 수 있다.

![image](https://github.com/user-attachments/assets/dc863cfa-8da2-475f-a352-66d00ffc9555)

-   게시글이 삭제되어 존재하지 않거나, 오류가 발생한면 위와 같은 응답이 돌아온다. 존재하는 게시글이라면 다음과 같은 응답이 돌아온다.

![image](https://github.com/user-attachments/assets/c3401dcf-c91a-472f-bbba-bb014bf4b0ce)

-   게시글 상세조회 기능은 **/getOneCommunity/:id** 를 통하여 조회를 진행한다.
-   이때 **id** 는 **CommunityId** 로 받아서 조회를 진행한다.
-   게시글 수정을 해보자.

![image](https://github.com/user-attachments/assets/ba989ae2-ec1f-4bf4-b946-f19eda56e0ef)

-   성공로그가 올라오긴 했지만, 게시글 상세조회로 잘 업데이트가 되었는지 한번 다시 확인해보자.

![image](https://github.com/user-attachments/assets/db34f30b-8e4c-45cf-90a0-0da0c9759b77)

-   게시글 수정 기능은 **/updateCommunity/:id** 를 통하여 수정을 진행한다.
-   이때 **id** 는 **CommunityId** 로 받아서 수정을 진행한다.
-   게시글 삭제를 해보자.

![image](https://github.com/user-attachments/assets/74f55b63-7623-4df2-8562-d116254032f7)

-   게시글 삭제 기능은 **/deleteCommunity/:id** 를 통하여 삭제를 진행한다.
-   이때 **id** 는 **CommunityId** 로 받아서 삭제를 진행한다.
-   전체조회를 해보면 삭제한 게시글이 성공적으로 없어진 것도 확인해볼 수 있다.
-   ![image](https://github.com/user-attachments/assets/981ff233-6447-4b93-af70-347a853ce08f)
-   이제 댓글 생성을 해보자.

![image](https://github.com/user-attachments/assets/6d58c75d-defa-48b5-9277-1cf5140e764a)

-   댓글 생성 기능은 **/createComment/:id** 를 통하여 생성을 진행한다.
-   이때 _id_ 는 **CommunityId** 로 받아서 생성을 진행한다.
-   여기서 **CommunityId** 는 **Foreign Key** 이다.
-   댓글 조회를 해보자. 조회 이전에 임의로 다른 댓글을 생성해보았다.

![image](https://github.com/user-attachments/assets/529248b3-ed6f-4b21-992a-18446db3b9c6)

-   댓글 조회 기능은 **/getComment/:id** 를 통하여 조회를 진행한다.
-   이때 _id_ 는 **CommunityId** 로 받아서 조회를 진행한다.
-   여기서 **CommunityId** 는 **Foreign Key** 이다.
-   댓글 수정을 해보자.

![image](https://github.com/user-attachments/assets/30016770-f957-424e-8ffb-c488cfa44c49)

-   댓글 수정 기능은 **/updateComment/:id** 를 통하여 수정을 진행한다.
-   이때 위 기능들과는 다르게 본 기능에서의 **id** 는 **CommentId** 로 받아서 수정을 진행한다. 이 **CommentId** 는 다른 게시글로 넘어가도 서로 유일하게 존재한다.
-   마지막으로 댓글 삭제를 해보자.

![image](https://github.com/user-attachments/assets/e99fb17d-797e-4db5-be25-915a58515bfc)

-   댓글 삭제 기능은 **/deleteComment/:id** 를 통하여 삭제를 진행한다.
-   이때 **id** 는 **CommentId** 로 받아서 삭제를 진행한다.

![image](https://github.com/user-attachments/assets/8d1a6fdd-5d76-4cbd-b25b-88788d29882d)

-   결과를 확인해보면 성공적으로 댓글이 삭제된 것을 확인할 수 있다.
