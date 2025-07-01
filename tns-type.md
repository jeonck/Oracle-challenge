## **TNS 연결 타입의 사용 이유와 설정 방법**

인텔리제이에서 오라클 데이터베이스에 연결할 때 **TNS**(Transparent Network Substrate) 연결 타입을 사용하는 이유는 다음과 같습니다:

### **1. 간편한 연결 관리**

TNS를 사용하면 데이터베이스 연결 정보를 **tnsnames.ora** 파일에 저장할 수 있습니다. 이 파일은 여러 데이터베이스에 대한 연결 정보를 포함하고 있어, 각 데이터베이스의 호스트, 포트, 서비스 이름 등을 일일이 입력할 필요 없이 간편하게 관리할 수 있습니다. 

### **2. 환경 설정의 일관성**

TNS를 사용하면 여러 개발자나 애플리케이션이 동일한 연결 정보를 공유할 수 있습니다. 이는 특히 팀 프로젝트에서 유용하며, 데이터베이스의 위치나 포트가 변경되더라도 **tnsnames.ora** 파일만 수정하면 되므로 유지보수가 용이합니다.

### **3. 복잡한 연결 문자열의 단순화**

TNS를 사용하면 복잡한 JDBC URL을 직접 입력할 필요 없이, 간단한 TNS 이름만으로 연결할 수 있습니다. 예를 들어, SID 방식으로 연결할 경우 `host:port:sid` 형식의 URL을 입력해야 하지만, TNS 방식에서는 `TNS_NAME`만 입력하면 됩니다.

### **TNS 연결 설정 방법**

1. **데이터 소스 추가**: 인텔리제이에서 데이터베이스 탭을 열고, **+ 버튼**을 클릭한 후 **Data Source > Oracle**을 선택합니다.

2. **연결 타입 선택**: 연결 타입을 **TNS**로 설정합니다.

3. **TNSADMIN 설정**: TNSADMIN 필드에 **tnsnames.ora** 파일이 위치한 디렉토리를 입력합니다.

4. **TNS 이름 입력**: TNS 이름 필드에 연결할 데이터베이스의 TNS 이름을 입력합니다. 이 이름은 **tnsnames.ora** 파일에서 정의된 서비스 이름과 일치해야 합니다.

5. **사용자 및 비밀번호 입력**: 데이터베이스에 접근하기 위한 사용자 이름과 비밀번호를 입력합니다.

6. **연결 테스트**: 모든 정보를 입력한 후 **Test Connection** 버튼을 클릭하여 연결이 성공적으로 이루어지는지 확인합니다.

이러한 과정을 통해 TNS 연결 타입을 사용하여 오라클 데이터베이스에 쉽게 연결할 수 있습니다. TNS 방식은 특히 여러 데이터베이스를 관리해야 하는 환경에서 유용하며, 연결 설정을 간소화하는 데 큰 도움이 됩니다.
[1] https://velog.io/@00yubin00/DB-%EC%9D%B8%ED%85%94%EB%A6%AC%EC%A0%9C%EC%9D%B4%EC%97%90%EC%84%9C-DB-%EC%97%B0%EA%B2%B0%ED%95%B4%EC%84%9C-%EC%9E%91%EC%84%B1%ED%95%98%EA%B8%B0
[2] https://creampuffy.tistory.com/105
[3] https://stackoverflow.com/questions/60350424/how-to-configure-intellij-idea-to-use-oracle-tnsnames
[4] https://ar-tec.tistory.com/55
[5] https://intellij-support.jetbrains.com/hc/en-us/community/posts/206634435-How-to-connect-to-Oracle-DB-with-TNS-File
[6] https://docs.oracle.com/en/database/oracle/oracle-database/18/ntcli/specifying-connection-by-configuring-tnsnames.ora-file.html
[7] https://intellij-support.jetbrains.com/hc/en-us/community/posts/205453310-Solved-Trouble-setting-up-Oracle-connections-using-tnsnames-ora
[8] https://ssdragon.tistory.com/47
[9] https://yongc.tistory.com/65
[10] https://intellij-support.jetbrains.com/hc/en-us/community/posts/203526830-Connect-Datagrip-to-Oracle-using-TNS
[11] https://www.jetbrains.com/help/idea/how-to-connect-to-oracle-with-oci.html
[12] https://intellij-support.jetbrains.com/hc/en-us/community/posts/360007637940-how-to-use-oracle-tnsnames-ora
[13] https://docs.oracle.com/ko/cloud/paas/autonomous-database/dedicated/cordu/index.html
[14] https://intellij-support.jetbrains.com/hc/en-us/community/posts/360005050860-How-to-setup-DataGrip-to-connect-to-Oracle-DB-using-sqlnet-ora
[15] http://m.blog.naver.com/km3957/221496746205
[16] https://velog.io/@fgh1937/22.09.02-Oracle-cloud-%EC%99%80-JDBC-%EC%97%B0%EA%B2%B0-%EB%AC%B8%EC%A0%9C
[17] https://docs.oracle.com/ko/cloud/paas/autonomous-database/dedicated/cordu/index.html?source=%3Aex%3Apw%3A%3A%3A%3A%3ATNS_SQL_FEB25_E
[18] https://bambookim.tistory.com/4
[19] https://docs.oracle.com/en/cloud/paas/autonomous-database/dedicated/cordu/index.html
[20] https://rebugs.tistory.com/553
[21] https://www.jetbrains.com/help/idea/oracle.html
[22] https://www.jetbrains.com/help/idea/connectivity-problems.html
[23] https://www.quora.com/How-do-I-connect-Java-with-an-Oracle-database-in-the-IntelliJ-Community
[24] https://intellij-support.jetbrains.com/hc/en-us/community/posts/360004135499-Oracle-Ldap
