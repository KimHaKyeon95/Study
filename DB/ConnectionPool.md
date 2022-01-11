2022/01/11

## Connection Pool
Connection을 생성하는 것은 자원을 많이 먹는 작업이다. 이전에는 매번 getConnection()을 통해 Connection객체를 생성하여 작업을 
했는데 이러한 Connection을 줄이기 위해 JOIN도 해보고 많은 추가적인 작업을 수행했다.  
자원낭비를 막기 위해 미리 Connection Pool에서 Connection을 생성할 수 있다. 작동원리는 웹 컨테이너가 실행되면 DB와 미리 
연결을 해둔 객체를 Pool에 저장하고 요청이 들어오면 연결(connection)을 빌려주고 처리가 끝나면 Pool에 저장을 한다. 이 때 
DataSource라는 인터페이스를 통해 Connection Pool을 구현하기 위한 스펙을 정한다.

## DataSource
DataSource는 Connection Pool을 구현하기 위한 스펙을 정의하는 인터페이스이다. 나는 DataSource를 사용해 Spring에서 DB와의 연결을 
사용하기 위해 다음과 같이 설정을 하였다.
```xml
  <bean id="dataSource"
		class="org.springframework.jdbc.datasource.SimpleDriverDataSource">
	<property name="driverClass"
		value="oracle.jdbc.driver.OracleDriver">
	</property>
	<property name="url" value="jdbc:oracle:thin:@localhost:1521:xe"></property>
	<property name="username" value="hr"></property>
	<property name="password" value="hr"></property>
  </bean>
```
DB의 IP와 포트번호를 localhost, 1521로 명시하고 username와 비밀번호를 명시하여 DB로 접속을 하도록 설정했다. 이렇게 설정된 DataSource를 
참조한 SqlSessionFactory라는 객체가 MyBatis와 OracleDB를 연결할 것이다.

## SqlSessionFactory
SqlSessionFactory는 데이터베이스 연결과 SQL실행에 대한 모든 것을 가진 객체이다. 이를 사용하기 위해선 우선 Bean객체로 등록해줘야 한다. 
```xml
	<bean id="sqlSessionFactory"
		class="org.mybatis.spring.SqlSessionFactoryBean">
	  <property name="dataSource" ref="dataSource"></property>
	  <property name="configLocation"
		  value="classpath:mybatis-config.xml">
	  </property>
	</bean>
```
ref="dataSource"를 통해 위에 설정해둔 DataSource를 참조하는 것을 볼 수 있다. 그리고 MyBatis의 설정을 위한 mybatis-config.xml가 
Spring이 동작할 때 같이 동작하도록 하기 위해 configLocation속성을 추가했다.
