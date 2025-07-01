Oracle TNS 설정에서 **retries**와 **delay** 옵션은 ADDRESS_LIST나 ADDRESS 블록에만 사용할 수 있습니다. 이 옵션들은 각각 연결 시도 횟수와 각 시도 사이의 대기 시간을 지정합니다. 아래는 **RETRY_COUNT**, **RETRY_DELAY**(또는 **retries**, **delay**) 옵션을 올바르게 포함한 예시입니다.

```ini
MYDB =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = primary_db)(PORT = 1521))
      (ADDRESS = (PROTOCOL = TCP)(HOST = backup_db)(PORT = 1521))
      (FAILOVER = ON)
      (LOAD_BALANCE = OFF)
      (RETRY_COUNT = 5)   -- 연결 재시도 횟수
      (RETRY_DELAY = 5)   -- 재시도 간 대기 시간(초)
      (RETRIES = 5)       -- 일부 Oracle 버전에서는 RETRIES 사용
      (DELAY = 5)         -- 일부 Oracle 버전에서는 DELAY 사용
    )
    (CONNECT_DATA =
      (SERVICE_NAME = my_service)
      (FAILOVER_MODE =
        (TYPE = SELECT)
        (METHOD = BASIC)
      )
    )
  )
```

- **RETRY_COUNT/RETRIES**: 연결 재시도 횟수
- **RETRY_DELAY/DELAY**: 각 재시도 사이의 대기 시간(초)

Oracle 버전에 따라 **RETRY_COUNT/RETRY_DELAY** 또는 **RETRIES/DELAY** 중 하나만 지원할 수 있으니, 실제 환경에 맞는 공식 문서를 참고해야 합니다.

**중복 설정은 피하고, 실제 사용하는 Oracle 버전에 맞는 옵션만 남기는 것이 좋습니다.** 예를 들어, 최신 버전에서는 RETRY_COUNT/RETRY_DELAY만 사용하거나, 구버전에서는 RETRIES/DELAY만 사용할 수 있습니다.

이렇게 ADDRESS_LIST 안에 옵션을 추가하면 올바르게 동작합니다. 추가적인 네트워크 구성이나 환경에 따라 더 세부적인 설정이 필요하다면 말씀해 주세요.
[1] https://stackoverflow.com/questions/39587530/spring-boot-how-to-set-retry-attempts-for-oracle-production-database-connection
[2] https://docs.oracle.com/cd/B28359_01/network.111/b28317/tnsnames.htm
[3] https://docs.oracle.com/database/121/NETRF/tnsnames.htm
[4] https://docs.oracle.com/en/database/oracle/oracle-database/21/netrf/local-naming-parameters-in-tns-ora-file.html
[5] https://node-oracledb.readthedocs.io/en/latest/user_guide/connection_handling.html
[6] https://docs.oracle.com/en/database/oracle/oracle-database/23/netrf/local-naming-parameters-in-tns-ora-file.html
[7] https://docs.oracle.com/database/122/NETAG/enabling-advanced-features.htm
[8] https://docs.progress.com/bundle/datadirect-oracle-odbc-80/page/TNSNames-File.html
[9] https://www.dbi-services.com/blog/transport_connect_timeout-and-retry_count/
[10] https://blog.naver.com/PostView.nhn?blogId=jyc8618&logNo=220163994820
[11] https://latale.tistory.com/372
[12] https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=darksaking&logNo=221084502496
[13] https://docs.oracle.com/en/database/oracle/oracle-database/23/haovw/oracle-net-tns-string-parameters.html
[14] https://epkim.tistory.com/47
[15] https://da-new.tistory.com/3
[16] https://yongblog.tistory.com/23
[17] https://blog.naver.com/young_0620/220796488171?viewType=pc
[18] https://uutopia.tistory.com/3
[19] https://docs.oracle.com/cd/E18283_01/network.112/e10835/tnsnames.htm
[20] https://bangu4.tistory.com/13
[21] https://kb.lenels2.com/home/what-is-the-tnsnamesora-file
[22] https://widm2240.tistory.com/64
[23] https://asktom.oracle.com/ords/f?p=100:11:109966378764103::::P11_QUESTION_ID:1281675938015
[24] https://docs.oracle.com/ko/solutions/adb-refreshable-clones-dr/configure-dr-topology.html
[25] https://docs.oracle.com/en/database/oracle/oracle-database/18/netrf/local-naming-parameters-in-tnsnames-ora-file.html
[26] https://dbeaver.com/docs/dbeaver/Connecting-to-Oracle-databases/
[27] http://blog.naver.com/absoliam/70101241775
[28] https://python-oracledb.readthedocs.io/en/latest/user_guide/connection_handling.html
[29] https://ohmyfun.tistory.com/391
[30] https://stackoverflow.com/questions/74917229/how-to-connect-to-oracle-database-with-multiple-address-line-tns-in-pentaho-data
[31] https://help.salesforce.com/s/articleView?id=001453730&language=ko&type=1
[32] https://forums.devart.com/viewtopic.php?t=33057
[33] https://docs.progress.com/bundle/datadirect-connect-jdbc-51/page/Configuring-the-tnsnames.ora-File.html
[34] https://stackoverflow.com/questions/15819433/difference-between-using-a-tns-name-and-a-service-name-in-a-jdbc-connection
[35] https://docs.progress.com/bundle/datadirect-oracle-odbc-80/page/Configuring-failover-using-the-TNSNAMES.ORA-file.html
[36] https://stackoverflow.com/questions/41424589/connection-to-oracle-via-tns-is-not-working
[37] https://docs.oracle.com/cd/B13789_01/network.101/b10776/tnsnames.htm
[38] https://nunbu.tistory.com/170
[39] https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.SSL.Connecting.html
[40] https://seop00.tistory.com/20
[41] https://documentation.vizrt.com/viz-pilot-guide/8.8/Oracle_Database.html
[42] https://querysurge.zendesk.com/hc/en-us/articles/115000379546-Configuring-Connections-Advanced-Oracle-Connection-Options
[43] https://docs.progress.com/bundle/datadirect-oracle-jdbc-60/page/Configuring-the-tnsnames.ora-file.html
[44] https://m.blog.naver.com/bestdriver94/220577359537
[45] https://epkim.tistory.com/31
[46] https://kr98gyeongim.tistory.com/94
[47] https://gyh214.tistory.com/191
[48] https://boeok.tistory.com/entry/Listener-Tnsnamesora-Sqlnetora-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0
[49] https://forums.oracle.com/ords/apexds/post/20-second-delay-in-tnsnames-authentication-2504
[50] http://www.pafumi.net/Optimize_Your_Oracle_Network_Configuration.html
[51] https://docs.oracle.com/cd/B19306_01/network.102/b14213/tnsnames.htm
