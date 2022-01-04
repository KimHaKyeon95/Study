## Spring 개요
JSP/Servlet을 사용해서 프로젝트를 진행하였을 때 몇가지 단점이 있었다.
* Servlet자체가 많아졌다.  
  이를 해결하기 위해서 FrontController를 도입하여 Servlet자체를 줄였지만  
  * properties파일을 여러개 만들고 이를 사용할 코드를 다 작성해야했다.  
  * 런타임다이나믹로드와 Reflection처리를 따로 해줘야했다.
* 또한 Controller를 규격화해서 사용해야했다. 이로 인해 유연한 개발이 힘들었고, 이는 코드간 강한 결합이 형성되었음을 의미했다.  
여러가지 개발 상의 문제를 해결하기 위해 새로운 기술을 사용할 필요가 있었다.  

## EJB(Enterprise JavaBean)
EJB는 기업형 분산 컴포넌트 기술을 의미한다. 이 기술은 자바로 엔터프라이즈 애플리케이션을 개발하는 일반적인 방법이었다.  

하지만 EJB는 단점이 있었는데,
1. 단위테스트가 어렵고
2. 예외 처리가 번거롭고
3. 기술을 구현하기 위한 특별한 엔진을 사용하기 위해 WAS(Web Application Server)를 비싼 가격을 주고 사용해야했다.
WAS는 EJB를 구동하기 위한 엔진을 포함하는 경우가 많았고 웹서버의 역할과 미들웨어의 역할을 해내며 JavaEE를 완벽하게 구현할 수 있으나 꽤나 높은 가격을 지녔다는 단점이 존재한다.
* 미들웨어 : 매개체간(클라이언트-서버, 서버-서버 등)을 연결하는 레이어로써, 통상적으로 웹/애플리케이션 서버를 의미한다.  
4. 학습곡선 기울기가 심하고 처리속도가 느리다.
5. 개발스펙이 규격화되어있기 때문에 유연하지 못하다.  

이런 단점들을 보완하기 위해서 **Spring**이 등장했다.  
Spring은 WAS가 필요없고 학습하기 쉬우며, 처리속도가 빠르고 개발스펙이 유연하다. Spring자체는 분산처리가 안되지만 Spring Cloud를 사용하면 분산처리 또한 가능하다.

## Spring
Spring은 자바 플랫폼을 위한 오픈소스 애플리케이션 프레임워크이다. Spring은 IoC기반으로 작동한다.  

### IoC(Inversion of Control), DI(Dependency Injection)
Spring은 Spring Container를 사용하여 요청이 들어왔을 때 객체(POJO)를 제어한다. 이를 제어하는 방법을 개발자는 따로 작성할 필요가 없다.
Spring Container가 대신 복잡한 처리를 수행해주기 때문이다.  
xml파일에 다음과 같은 요소를 추가했다고 가정해보자.  
```xml
<bean id="a" class="com.my.customer.dao.CustomerDAOOracle"/>
```
Spring Container가 구동되면 연결된 xml파일을 찾고 bean태그의 클래스 속성의 값을 객체로 생성해낸다. 여기서 CustomerDAOOracle은 CustomerDAO라는 인터페이스를 사용한다.   
위와 같이 bean에 의해 외부 파일에서 의존성(Dependency)를 정의하고 Container가 외부 파일로부터 의존성을 공급받는 것을 **DI(Dependency Injection)** 이라고 한다.
객체를 사용하려면 다음과 같이 사용할 수 있다.
```java
ctx=~~;
CustomerDAO dao = ctx.getBean("a", CustomerDAO.class);
```
getBean은 a라는 이름의 객체가 CustomerDAO을 참조하는 변수인지를 확인하는데 xml에서 의존성을 정의한 바에 의하면 a는 CustomerDAOOracle을 참조하는 변수이다.  
CustomerDAOOracle은 CustomerDAO를 상속받기 때문에 getBean에서 찾는 객체로 인식이 된다.  
만약, CustomerDAO를 상속받는 CustomerDAOMySQL이라는 객체가 존재하고 xml에서 설정을 id=a이고 class=com.my.customer.dao.CustomerDAOMySQL이라고 할 때, 
getBean("a", CustomerDAO.class)를 실행시키면 CustomerDAOMySQL를 참조하는 a변수를 찾게된다. 이렇게 xml에 정의된 바에 따라 사용자가 아닌 Spring Container가
제어를 가져가게 되고 이것을 **IoC(Inversino of Control)** 이라고 하는 것이다.  
**즉. IoC는 의존성을 외부에서 정의하고 컨테이너에 의해 해당 의존성을 공급받는 것을 의미한다. DI는 IoC의 하위 개념이 된다.**
