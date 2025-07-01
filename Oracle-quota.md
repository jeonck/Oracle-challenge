오라클에서 **테이블스페이스**는 데이터베이스 객체(테이블, 인덱스 등)가 실제로 저장되는 논리적 저장 공간입니다. 여러 개의 데이터 파일로 구성될 수 있으며, 데이터베이스의 저장 구조를 논리적으로 구분하는 가장 상위 단위입니다[1][2].

**유저(사용자) 중심의 테이블스페이스 개념**
- 오라클의 **유저**는 데이터베이스 내에서 객체(테이블, 뷰 등)를 생성하고 관리할 수 있는 계정입니다.
- 각 유저는 **기본 테이블스페이스**(default tablespace)를 할당받으며, 이 공간에 자신의 테이블이나 인덱스 등 객체를 생성합니다[3][1].
- 유저가 테이블을 생성하면, 그 데이터는 할당된 테이블스페이스에 저장됩니다. 만약 별도로 지정하지 않으면, 유저의 기본 테이블스페이스에 저장됩니다[3][2].
- 여러 유저가 같은 테이블스페이스를 사용할 수 있지만, 각 유저의 객체는 자신의 스키마(=유저명) 아래에 생성되어 서로 구분됩니다[3].

**쿼타(Quota)와 테이블 생성 과정**
- **쿼타(Quota)**란, 특정 유저가 특정 테이블스페이스에서 사용할 수 있는 **최대 저장 용량**을 제한하는 설정입니다[4].
- 유저에게 쿼타가 할당되지 않으면, 해당 테이블스페이스에 객체를 생성할 수 없습니다. 예를 들어, 유저의 default tablespace가 USERS이지만, quota=0이면 USERS 테이블스페이스에 테이블을 생성할 수 없습니다[4].
- 쿼타는 유저별, 테이블스페이스별로 다르게 설정할 수 있습니다. 예시:
  ```sql
  ALTER USER 사용자명 QUOTA 10M ON 테이블스페이스명;
  ```
  또는 무제한으로 할당할 수도 있습니다:
  ```sql
  ALTER USER 사용자명 QUOTA UNLIMITED ON 테이블스페이스명;
  ```
- 쿼타가 설정된 후, 유저는 해당 테이블스페이스 내에서만 주어진 용량 한도 내에서 테이블을 생성할 수 있습니다[4][2].

**테이블 생성 예시(유저 관점, 쿼타 강조)**
1. DBA가 테이블스페이스를 생성합니다.
   ```sql
   CREATE TABLESPACE MYTS DATAFILE '/경로/MYTS01.dbf' SIZE 100M;
   ```
2. 유저를 생성하며, 기본 테이블스페이스와 쿼타를 지정합니다.
   ```sql
   CREATE USER ora_user IDENTIFIED BY 비밀번호
     DEFAULT TABLESPACE MYTS
     QUOTA 20M ON MYTS;
   ```
3. 유저에게 권한을 부여합니다.
   ```sql
   GRANT CONNECT, RESOURCE TO ora_user;
   ```
4. ora_user로 로그인한 후, 테이블을 생성하면 데이터는 MYTS 테이블스페이스에 저장되고, 20MB 쿼타 내에서만 저장이 가능합니다.
   ```sql
   CREATE TABLE test_table (
     id NUMBER PRIMARY KEY,
     name VARCHAR2(50)
   );
   ```
   만약 20MB를 초과하는 데이터를 저장하려 하면, "공간 부족" 오류가 발생합니다[4][2].

**정리**
- **테이블스페이스**는 논리적 저장 공간, **유저**는 객체를 생성하는 계정, **쿼타**는 유저가 테이블스페이스에서 사용할 수 있는 용량 한도입니다.
- 유저는 할당된 테이블스페이스와 쿼타 내에서만 테이블을 생성하고 데이터를 저장할 수 있습니다[4][2].

출처
[1] [Oracle] 테이블 스페이스 생성 및 사용자 생성 - what is programming https://prinha.tistory.com/entry/Oracle-%ED%85%8C%EC%9D%B4%EB%B8%94-%EC%8A%A4%ED%8E%98%EC%9D%B4%EC%8A%A4-%EC%83%9D%EC%84%B1-%EB%B0%8F-%EC%82%AC%EC%9A%A9%EC%9E%90-%EC%83%9D%EC%84%B1
[2] [DB] Oracle 테이블스페이스 및 유저 생성부터 등록 https://velog.io/@www_j/DB-Oracle-%ED%85%8C%EC%9D%B4%EB%B8%94%EC%8A%A4%ED%8E%98%EC%9D%B4%EC%8A%A4-%EB%B0%8F-%EC%9C%A0%EC%A0%80-%EC%83%9D%EC%84%B1%EB%B6%80%ED%84%B0-%EB%93%B1%EB%A1%9D
[3] ORACLE의 TABLESPACE, SCHEMA, USER 관계 https://kcoc.tistory.com/33
[4] [Oracle] User별 Quota 확인 및 변경(+ Unlimited Tablespace) https://gayoung78.tistory.com/84
[5] ORACLE 테이블 생성/수정/삭제/복사 - 디노프랭키 - 티스토리 https://mystyle70024.tistory.com/9
[6] [Oracle] 오라클 테이블 스페이스 사용법(조회, 생성, 삭제) https://roeldowney.tistory.com/616
[7] [Oracle] 테이블 스페이스 설정 - 덕질st 공순이 - 티스토리 https://joy52.tistory.com/160
[8] [SQL-오라클] 테이블 생성 create, 구조 변경 alter, 삭제 drop, 주석 추가 https://kio15978.tistory.com/4
[9] 오라클 데이터베이스 관리자 기초- (9)사용자 관리 https://alljbut.tistory.com/85
[10] [DB] Oracle TABLESPACE 란 - 유혁의 개발 스토리 - 티스토리 https://yoo-hyeok.tistory.com/136
[11] Oracle 테이블스페이스(TableSpace) 개념 정리 - 환's World https://valuableinfo.tistory.com/26
[12] [Oracle] 오라클 테이블 스페이스 사용법(조회, 생성, 삭제)등 ... https://nizimo.tistory.com/327
[13] [DataBase] Oracle 문법(계정 생성과 권한 부여,취소,조회 ... https://spidyweb.tistory.com/62
[14] [Oracle] 유저 및 테이블의 테이블스페이스 변경. https://smoh.tistory.com/145
[15] [Oracle SQL] DDL - 테이블 생성(CREATE), 제약 조건 - Amy IT https://amy-it.tistory.com/21
[16] [SQL/ORACLE] 테이블 생성 및 데이터 타입 - velog https://velog.io/@dani0817/SQL-%ED%85%8C%EC%9D%B4%EB%B8%94-%EC%83%9D%EC%84%B1-%EB%B0%8F-%EB%8D%B0%EC%9D%B4%ED%84%B0-%ED%83%80%EC%9E%85
[17] [SQL 22] 테이블 생성과 삭제, 데이터 타입 - 냉유's Log - 티스토리 https://keep-cool.tistory.com/49
[18] Oracle / PLSQL : CREATE TABLE 문 - Riz.Dev - 티스토리 https://rizdev.tistory.com/entry/PLSQL-CREATE-TABLE
[19] [DB/오라클] 테이블 생성 시 주의사항 (테이블 작성 방법) https://codingwone.tistory.com/30
[20] [데이터베이스 운영] DBMS 기초 «수업-3» : 데이터베이스 및 테이블 ... https://920416.tistory.com/69
