Oracle에서 로컬 사용자 계정을 생성할 때도 `c##`을 붙여야 하는 이유는 Oracle 12c 버전부터 도입된 **CDB (Container Database)**와 **PDB (Pluggable Database)** 구조 때문입니다. 

### **CDB와 PDB 개념**

- **CDB**: 여러 개의 PDB를 포함할 수 있는 데이터베이스입니다. CDB는 데이터베이스의 관리 및 리소스 할당을 중앙에서 처리합니다.
  
- **PDB**: 독립적으로 운영될 수 있는 데이터베이스 인스턴스입니다. 각 PDB는 자체적인 사용자, 테이블, 데이터 등을 가질 수 있습니다.

### **c## 접두사의 필요성**

1. **공통 사용자(Common User)**: CDB에서 생성되는 사용자로, 이름이 `c##` 또는 `C##`로 시작해야 합니다. 이는 해당 사용자가 CDB 내에서 여러 PDB에 접근할 수 있는 권한을 가진 공통 사용자임을 나타냅니다[1][2][5].

2. **로컬 사용자(Local User)**: PDB 내에서만 존재하며, 이 경우 `c##` 접두사를 사용할 필요가 없습니다. 그러나 CDB의 루트 컨테이너에서 사용자 계정을 생성하려고 할 경우, 기본적으로 공통 사용자로 간주되기 때문에 `c##`을 붙여야 합니다[4][11].

### **해결 방법**

- 만약 `c##` 없이 로컬 사용자를 생성하고 싶다면, PDB에 직접 접속하여 사용자 계정을 생성해야 합니다. 또는, 세션에서 `_ORACLE_SCRIPT`를 `true`로 설정하여 공통 사용자 규칙을 우회할 수 있습니다:

```sql
ALTER SESSION SET "_ORACLE_SCRIPT" = TRUE;
CREATE USER 사용자명 IDENTIFIED BY 비밀번호;
```

이렇게 하면 `c##` 없이도 사용자 계정을 생성할 수 있습니다[3][5][6].

### **결론**

따라서, Oracle 12c에서 로컬 사용자 계정을 생성할 때 `c##`을 붙여야 하는 이유는 CDB의 구조와 관련이 있으며, 이를 통해 공통 사용자와 로컬 사용자의 구분이 이루어집니다.
[1] https://gent.tistory.com/528
[2] https://www.dbasolved.com/2013/06/common-user-vs-local-user-12c-edition/
[3] https://dbrang.tistory.com/1702
[4] https://docs.oracle.com/en/database/oracle/oracle-database/19/sqlrf/CREATE-USER.html
[5] https://velog.io/@seowj0710/Oracle-%EC%82%AC%EC%9A%A9%EC%9E%90-%EC%83%9D%EC%84%B1-%EB%B0%8F-%EA%B6%8C%ED%95%9C-%EC%84%A4%EC%A0%95
[6] https://yunassnn.tistory.com/40
[7] https://stackoverflow.com/questions/62023693/oracle-database-different-between-c-user-and-regular-user
[8] https://forums.oracle.com/ords/apexds/post/issue-creating-user-in-12c-database-5086
[9] https://dba.stackexchange.com/questions/129098/why-have-to-write-c-for-user-creation-in-oracle-12c
[10] https://stackoverflow.com/questions/61838855/regarding-oracle-database-sysdba-user-and-creating-a-new-user
[11] https://emersontech.com.br/en/create-user-without-c-prefix/
[12] https://www.reddit.com/r/oracle/comments/1hwxg4g/how_to_create_a_user/
[13] https://docs.oracle.com/en/database/oracle/oracle-database/21/refrn/COMMON_USER_PREFIX.html
[14] https://www.dbi-services.com/blog/any-reason-to-change-the-c-common-user-prefix/
[15] https://docs.netapp.com/ko-kr/netapp-solutions/databases/aws_ora_ha_pacemaker.html
[16] https://cloud.google.com/dataplex/docs/develop-custom-connector?hl=ko
