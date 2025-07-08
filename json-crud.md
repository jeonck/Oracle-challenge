## Oracle DB에서 JSON 데이터 저장과 활용

Oracle Database는 JSON 데이터를 효율적으로 저장하고 활용할 수 있는 다양한 기능을 제공합니다. JSON 데이터는 관계형 데이터와 함께 사용할 수 있으며, 스키마 없이도 저장, 인덱스화 및 쿼리할 수 있는 장점이 있습니다.

**JSON 데이터 저장 방법**

1. **데이터 타입**: Oracle Database에서는 JSON 데이터를 `VARCHAR2`, `CLOB`, `BLOB`와 같은 표준 SQL 데이터 타입으로 저장할 수 있습니다. JSON 데이터의 크기에 따라 적절한 데이터 타입을 선택하는 것이 중요합니다.
   - `VARCHAR2(4000)`: 최대 4000 바이트의 JSON 문서에 적합합니다.
   - `CLOB`: 더 큰 JSON 문서에 적합하며, 최대 4GB까지 저장할 수 있습니다.
   - `BLOB`: 이진 형식으로 JSON 데이터를 저장할 수 있으며, 문자 집합 변환이 필요 없습니다[1][9].

2. **JSON 열 생성**: JSON 데이터를 저장할 열을 생성할 때는 `IS JSON` 제약 조건을 사용하는 것이 좋습니다. 이를 통해 열의 값이 유효한 JSON 형식인지 확인할 수 있습니다. 예를 들어, 다음과 같은 SQL 문을 사용할 수 있습니다:

   ```sql
   CREATE TABLE example_table (
       id NUMBER,
       json_data CLOB CONSTRAINT json_check CHECK (json_data IS JSON)
   );
   ```

**JSON 데이터 활용 방법**

1. **데이터 삽입**: JSON 데이터를 삽입할 때는 일반적인 `INSERT` 문을 사용할 수 있습니다. 예를 들어:

   ```sql
   INSERT INTO example_table (id, json_data) 
   VALUES (1, '{"name": "John", "age": 30}');
   ```

2. **데이터 쿼리**: JSON 데이터를 쿼리할 때는 SQL/JSON 함수인 `JSON_VALUE`, `JSON_QUERY`, `JSON_TABLE` 등을 사용할 수 있습니다. 예를 들어, 특정 JSON 필드를 추출하는 쿼리는 다음과 같습니다:

   ```sql
   SELECT JSON_VALUE(json_data, '$.name') AS name 
   FROM example_table 
   WHERE id = 1;
   ```

3. **인덱싱**: JSON 데이터에 대한 쿼리 성능을 향상시키기 위해 인덱스를 생성할 수 있습니다. JSON 데이터에 대한 인덱스를 생성하면 검색 속도가 빨라집니다[1][3].

4. **SODA API 사용**: Oracle의 SODA (Simple Oracle Document Access) API를 사용하여 JSON 데이터를 RESTful 방식으로 처리할 수 있습니다. 이를 통해 다양한 프로그래밍 언어에서 JSON 데이터를 쉽게 관리할 수 있습니다[1][3].

Oracle Database는 JSON 데이터의 저장과 쿼리를 위한 강력한 기능을 제공하여, 개발자들이 관계형 데이터와 JSON 데이터를 통합하여 사용할 수 있도록 지원합니다. 이러한 기능을 활용하면 데이터베이스의 유연성과 성능을 극대화할 수 있습니다.
[1] https://docs.oracle.com/en/database/oracle/oracle-database/18/adjsn/json-in-oracle-database.html
[2] https://medium.com/@anders.swanson.93/json-in-oracle-database-from-19c-to-23ai-adbfbdb6789c
[3] https://www.oracle.com/nz/database/technologies/appdev/json.html
[4] https://shiningsense.com/1892
[5] https://medium.com/@npaberin/json-java-and-the-oracle-database-part-1-86d242fbce15
[6] https://docs.aws.amazon.com/ko_kr/prescriptive-guidance/latest/patterns/convert-json-oracle-queries-into-postgresql-database-sql.html
[7] https://stackoverflow.com/questions/29235854/keep-json-in-oracle-database
[8] https://bluebyte.tistory.com/89
[9] https://docs.oracle.com/en/database/oracle/oracle-database/21/adjsn/overview-of-storage-and-management-of-JSON-data.html
[10] https://oracle-base.com/articles/21c/json-data-type-21c
[11] https://oracle-cloud.tistory.com/entry/%EC%98%A4%EB%9D%BC%ED%81%B4-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-JSON-%EC%A7%80%EC%9B%90
[12] https://docs.oracle.com/en/database/oracle/oracle-database/18/adjsn/overview-of-storage-and-management-of-JSON-data.html
[13] https://docs.oracle.com/en/database/oracle/oracle-database/23/adjsn/json-in-oracle-database.html
[14] https://python-oracledb.readthedocs.io/en/latest/user_guide/json_data_type.html
[15] https://docs.oracle.com/ko/cloud/paas/autonomous-database/dedicated/ujsfn/
[16] https://www.oracle.com/kr/database/what-is-json/
[17] https://www.oracle.com/kr/autonomous-database/autonomous-json-database/get-started/
[18] https://oracle-cloud.tistory.com/entry/%EC%98%A4%EB%9D%BC%ED%81%B4-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-23c-JSON-Relational-Duality-Views
[19] https://www.oracle.com/kr/autonomous-database/autonomous-json-database/
[20] https://blog.naver.com/pino93/222071912583
[21] https://velog.io/@effirin/DB%EC%97%90-JSON-%EC%A0%80%EC%9E%A5%ED%95%98%EA%B8%B0
[22] https://qodbtn.tistory.com/184
[23] https://katfun.tistory.com/224
[24] https://stackoverflow.com/questions/77191295/json-data-extraction-using-oracle-sql
[25] https://forums.oracle.com/ords/apexds/post/how-do-i-use-json-functions-like-json-extract-and-json-unqu-5518
[26] https://node-oracledb.readthedocs.io/en/v6.5.1/user_guide/json_data_type.html
[27] https://python-oracledb.readthedocs.io/en/v1.4.2/user_guide/json_data_type.html
[28] https://blogs.oracle.com/sql/post/how-to-store-query-and-create-json-documents-in-oracle-database
[29] https://www.reddit.com/r/dataengineering/comments/15k4oyg/storing_json_database_vs_object_storage/
[30] https://docs.oracle.com/en/database/oracle/oracle-database/21/adjsn/store-and-manage-json-data.html
[31] https://www.reddit.com/r/rails/comments/kg6k54/is_it_bad_practice_to_store_and_retrieve_data_in/


## Oracle DB에서 JSON 데이터의 CRUD 예시

Oracle Database에서 JSON 데이터를 관리하는 CRUD(Create, Read, Update, Delete) 작업은 다양한 SQL 명령어와 JSON 관련 함수를 통해 수행할 수 있습니다. 아래는 각 작업에 대한 설명과 예시입니다.

### **1. Create (생성)**

JSON 데이터를 데이터베이스에 삽입하는 작업입니다. `INSERT` 문을 사용하여 새로운 JSON 레코드를 추가할 수 있습니다.

```sql
INSERT INTO json_table (id, json_data) 
VALUES (1, '{"name": "Alice", "age": 30, "city": "New York"}');
```

위의 예시에서는 `json_table`이라는 테이블에 새로운 JSON 객체를 삽입합니다.

### **2. Read (조회)**

저장된 JSON 데이터를 조회하는 작업입니다. `SELECT` 문과 JSON 관련 함수를 사용하여 특정 필드를 추출할 수 있습니다.

```sql
SELECT JSON_VALUE(json_data, '$.name') AS name 
FROM json_table 
WHERE id = 1;
```

이 쿼리는 `json_table`에서 ID가 1인 레코드의 `name` 필드를 조회합니다.

### **3. Update (수정)**

기존 JSON 데이터를 수정하는 작업입니다. `UPDATE` 문과 JSON 관련 함수를 사용하여 특정 필드를 업데이트할 수 있습니다.

```sql
UPDATE json_table 
SET json_data = JSON_MERGEPATCH(json_data, '{"age": 31}') 
WHERE id = 1;
```

위의 예시는 ID가 1인 레코드의 `age` 필드를 31로 업데이트합니다.

### **4. Delete (삭제)**

저장된 JSON 데이터를 삭제하는 작업입니다. `DELETE` 문을 사용하여 특정 레코드를 제거할 수 있습니다.

```sql
DELETE FROM json_table 
WHERE id = 1;
```

이 쿼리는 ID가 1인 레코드를 `json_table`에서 삭제합니다.

### **JSON 데이터의 CRUD 작업 요약**

- **Create**: `INSERT` 문을 사용하여 JSON 객체를 추가합니다.
- **Read**: `SELECT` 문과 JSON 함수를 사용하여 JSON 필드를 조회합니다.
- **Update**: `UPDATE` 문과 JSON 함수를 사용하여 JSON 데이터를 수정합니다.
- **Delete**: `DELETE` 문을 사용하여 JSON 레코드를 삭제합니다.

이러한 CRUD 작업을 통해 Oracle Database에서 JSON 데이터를 효과적으로 관리할 수 있습니다. JSON 데이터는 관계형 데이터와 함께 사용할 수 있으며, 다양한 SQL 기능을 활용하여 복잡한 쿼리와 데이터 조작이 가능합니다.
[1] https://support.quest.com/it-it/technical-documents/shareplex/11.4/%EC%84%A4%EC%B9%98-%EB%B0%8F-%EC%84%A4%EC%A0%95-%EC%95%88%EB%82%B4%EC%84%9C/target
[2] https://docs.oracle.com/ko/cloud/paas/nosql-cloud/uakik/
[3] https://cloud.google.com/bigquery/docs/samples/bigquery-load-table-gcs-json-truncate?hl=ko
[4] https://www.oracle.com/kr/autonomous-database/autonomous-json-database/get-started/
[5] https://velog.io/@kimyun111/Oracle-DB-CRUD
[6] https://oracle-cloud.tistory.com/entry/%EC%98%A4%EB%9D%BC%ED%81%B4-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-JSON-%EC%A7%80%EC%9B%90
[7] https://cloud.google.com/spanner/docs/working-with-json?hl=ko
[8] https://smarttechways.com/2023/10/30/update-delete-and-modify-json-data-in-the-json-column-in-oracle/
[9] https://velog.io/@b1rdn2w/%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%ED%98%91%EC%97%85-Spring-Boot-%EC%84%9C%EB%B2%84-%EA%B5%AC%EC%B6%95-2-DB-%EC%A0%81%EC%9A%A9%EA%B3%BC-%ED%99%9C%EC%9A%A9
[10] https://docs.oracle.com/en/middleware/developer-tools/jet/9/develop/create-crud-application-using-oracle-jet.html
[11] https://oracle-cloud.tistory.com/entry/%EC%98%A4%EB%9D%BC%ED%81%B4-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-23c-JSON-Relational-Duality-Views
[12] https://winterrosy.tistory.com/4
[13] https://docs.oracle.com/ko/cloud/paas/autonomous-database/dedicated/adbbj/index.html?source=%3Aow%3Alp%3Acpo%3A%3A%3A%3ARC_CORP230307P00009%3ALPD400280972+%3Aow%3Alp%3Acpo%3A%3A%3A%3ARC_CORP231202P00004%3ADMO400323615
[14] https://docs.aws.amazon.com/ko_kr/prescriptive-guidance/latest/patterns/convert-json-oracle-queries-into-postgresql-database-sql.html
[15] https://docs.oracle.com/ko/learn/oci-bulk-delete-objects/
[16] https://kosate.github.io/blog/oracle/introducing-sqlcl/
[17] https://docs.oracle.com/en/database/oracle/oracle-database/23/adjsn/json_transform-operator-remove.html
[18] https://stackoverflow.com/questions/72842342/update-json-data-in-oracle-sql
[19] https://medium.com/db-one/level-up-your-productivity-with-json-in-oracle-database-80207e8dd16
[20] https://forums.oracle.com/ords/apexds/post/json-data-crud-operations-2683
[21] https://forums.oracle.com/ords/apexds/post/apex-24-1-generate-data-and-deploy-master-detail-crud-using-8981
[22] https://docs.oracle.com/ko/cloud/paas/autonomous-database/dedicated/ujsfn/
[23] https://docs.aws.amazon.com/ko_kr/dms/latest/userguide/CHAP_Troubleshooting.html
[24] https://blogs.oracle.com/sql/post/how-to-store-query-and-create-json-documents-in-oracle-database
[25] https://python-oracledb.readthedocs.io/en/latest/user_guide/json_data_type.html
[26] https://medium.com/@npaberin/json-java-and-the-oracle-database-part-1-86d242fbce15
[27] https://shiningsense.com/1892
[28] https://docs.oracle.com/en/database/oracle/oracle-database/21/adjsn/insert-load-and-update-json-data.html
[29] https://oracle-base.com/articles/21c/json-data-type-21c
[30] https://stackoverflow.com/questions/64702928/how-do-i-insert-json-data-into-an-oracle-database
[31] https://docs.oracle.com/en/database/oracle/oracle-database/19/adjsn/updating-json-document-json-merge-patch.html
[32] https://cloud.google.com/bigquery/docs/json-data?hl=ko
[33] https://blogs.oracle.com/korea/post/oracle-23ai-now-generally-available-kr
[34] https://docs.oracle.com/en/database/oracle/oracle-database/19/adjsn/overview-of-inserting-updating-loading-JSON-data.html
[35] https://forums.oracle.com/ords/apexds/post/generate-master-detail-data-in-json-format-4153
[36] https://martinelli.ch/oracle-json-relational-duality-views-with-spring-boot/
[37] https://docs.oracle.com/en/database/oracle/oracle-database/21/adjsn/json-in-oracle-database.html
[38] https://docs.aws.amazon.com/ko_kr/athena/latest/ug/parsing-json-data.html
[39] https://pretius.com/blog/parse-json-oracle-db/
[40] https://stackoverflow.com/questions/62508569/oracle-reading-json-data-using-json-query
[41] https://www.oracle.com/kr/database/what-is-json/
[42] https://www.oracle.com/kr/autonomous-database/autonomous-json-database/
[43] https://docs.oracle.com/en/cloud/paas/autonomous-database/serverless/adbsb/about-autonomous-database-workloads.html
[44] https://livelabs.oracle.com/pls/apex/r/dbpm/livelabs/view-workshop?wid=736
[45] https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Troubleshooting.html
[46] https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.PostgreSQL.html
[47] https://apex.oracle.com/ko/platform/features/whats-new-221/
