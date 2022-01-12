
2022/01/12

## HikariCP
DB Connection Pool로써 높은 성능을 자랑한다. 정해진 수만큼의 Connection을 Connection Pool에 넣어두고 요청이 들어오면 스레드가 요청한 Connection을 위해 Connection Pool에서 Connecntion을 연결해준다. Spring Boot 2.0부턴 기본 DBCP이다.
