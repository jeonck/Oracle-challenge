오라클에서 **관리자(어드민)가 생성한 테이블에 앱 계정(일반 사용자 계정)이 접근**하려면, 다음과 같은 절차를 따라야 합니다. 이 과정에서 핵심은 **객체 권한(SELECT, INSERT 등)**을 부여하는 것입니다.

**1. 어드민 계정이 테이블 생성**
- 예를 들어, 어드민 계정(admin)이 다음과 같이 테이블을 생성합니다.
  ```sql
  CREATE TABLE admin.table1 (
    id NUMBER PRIMARY KEY,
    name VARCHAR2(100)
  );
  ```

**2. 앱 계정 생성 및 기본 권한 부여**
- 앱 계정(app_user)이 없다면, DBA가 계정을 생성하고 최소한의 시스템 권한(예: CREATE SESSION)을 부여해야 합니다.
  ```sql
  CREATE USER app_user IDENTIFIED BY 비밀번호;
  GRANT CREATE SESSION TO app_user;
  ```
  - CREATE SESSION: 데이터베이스 접속 권한[5][4].

**3. 어드민 계정에서 객체 권한 부여**
- 어드민 계정이 **app_user**에게 자신의 테이블에 대한 접근 권한을 부여합니다.
- 필요한 권한(예: SELECT, INSERT, UPDATE, DELETE 등)에 따라 다음과 같이 실행합니다.
  ```sql
  GRANT SELECT, INSERT, UPDATE, DELETE ON admin.table1 TO app_user;
  ```
  - 이 명령은 app_user가 admin.table1 테이블을 조회, 입력, 수정, 삭제할 수 있게 합니다[5][4].
  - 만약 읽기만 허용하려면 `GRANT SELECT ON admin.table1 TO app_user;`로 제한할 수 있습니다.

**4. 앱 계정에서 테이블 접근**
- 이제 app_user는 자신의 계정으로 접속하여 admin.table1에 접근할 수 있습니다.
  - 예시:
    ```sql
    SELECT * FROM admin.table1;
    ```
  - 테이블 이름 앞에 반드시 소유자(스키마)명을 붙여서 `admin.table1` 형태로 접근해야 합니다.

**5. (선택) WITH GRANT OPTION**
- app_user가 다른 사용자에게도 권한을 부여할 수 있도록 하려면, 다음과 같이 옵션을 추가할 수 있습니다.
  ```sql
  GRANT SELECT ON admin.table1 TO app_user WITH GRANT OPTION;
  ```
  - 일반적으로 보안상 신중하게 사용해야 합니다[5].

**정리 표**

| 단계 | 주체         | 작업 내용                                                |
|------|--------------|---------------------------------------------------------|
| 1    | 어드민       | 테이블 생성 (admin.table1)                              |
| 2    | DBA/어드민   | 앱 계정 생성 및 CREATE SESSION 권한 부여                |
| 3    | 어드민       | 앱 계정에 테이블 객체 권한(SELECT 등) GRANT             |
| 4    | 앱 계정      | admin.table1에 접근 (SELECT, INSERT 등)                 |
| 5    | (선택) 어드민| WITH GRANT OPTION 등 추가 권한 부여                     |

**참고 사항**
- 권한은 필요 최소한만 부여하는 것이 보안상 안전합니다.
- 권한 부여 후 불필요해지면 REVOKE 명령으로 회수할 수 있습니다[5].
- 시스템 권한과 객체 권한을 구분하여 관리해야 하며, 객체 권한이 있어야만 타 계정의 테이블에 접근할 수 있습니다[5][4][6].

이 절차를 따르면 앱 계정이 어드민이 만든 테이블에 안전하게 접근할 수 있습니다.

출처
[1] [Oracle] 사용자 계정 생성 및 접속 권한 부여 - We Can it https://wecanit.tistory.com/88
[2] [oracle] 오라클 설치 후 사용자 계정 만들기/권한 부여 - hitomis https://hitomis.tistory.com/33
[3] [Oracle] SQL plus 에서 계정 등록 및 권한 설정 - zeroco - 티스토리 https://zeroco.tistory.com/22
[4] [Oracle] 오라클 DB 사용자 관리, 스키마란? (사용자 생성, 권한 ... https://hyunki99.tistory.com/54
[5] 오라클(Oracle) 사용자 계정, 권한 관리 필수 가이드, 예시 https://www.it-server-room.com/%EC%98%A4%EB%9D%BC%ED%81%B4-%EC%82%AC%EC%9A%A9%EC%9E%90-%EA%B3%84%EC%A0%95-%EA%B6%8C%ED%95%9C-%EA%B4%80%EB%A6%AC/
[6] [ORACLE] 오라클에서 사용자 권한 부여 및 회수 정리 (DCL) https://viera.tistory.com/9
[7] [ Database ] 오라클에서 관리자 접속하기 https://velog.io/@duck-ach/%EC%98%A4%EB%9D%BC%ED%81%B4%EC%97%90%EC%84%9C-%EA%B4%80%EB%A6%AC%EC%9E%90-%EC%A0%91%EC%86%8D%ED%95%98%EA%B8%B0
[8] 4 사용자, 액세스 역할 및 권한 설정 https://docs.oracle.com/ko/cloud/paas/blockchain-cloud/administeroci/set-users-and-application-roles.html
