## IoC는 왜 중요할까
IoC(Inversion of Control)의 핵심은 컴포넌트(시스템에서 독립적인 업무나 독립적인 기능을 수행하는 모듈)를 직접 생성하지 않고 의존성을 외부에 정의하여 컨테이너에 의해 공급받는 것이다.
이런 방식을 사용하면 클래스와 컴포넌트의 재사용성이 증가한다는 장점이 있다.  
사용 예시를 보면 다음과 같다.
```java
    String configLocation = "C:\\230\\mySpring\\di\\src\\config.xml";
		ApplicationContext context = new FileSystemXmlApplicationContext(configLocation);
		
		ProductDAOInterface dao = context.getBean("pDAO", ProductDAOInterface.class);
    System.out.println(dao == dao1);
    System.out.println(dao);
```
```xml
<!-- 	<bean id="pDAO"  class="com.my.product.dao.ProductDAOOracle"></bean> -->
	<context:component-scan base-package="com.my.product.dao"></context:component-scan>
```
config.xml파일에 context:component-scan을 통해 디렉토리를 검사하고 @Configure나 그 하위 어노테이션(@Service, @Controller,
@Repository)들이 달린 클래스를 스캔하여 Bean으로 등록해준다. 이와 같이 등록을 해주면 ProductDAOInterface를 사용하는 ProductDAOOracle은 
싱글톤 패턴으로 객체가 생성된다. 다른 이름을 사용해도 같은 객체를 사용하게 되는 것이다. 그렇다면 어디서 의존성이 줄어들었다는 것을
판단할 수 있을까?  
  
우선 Bean에 등록된 객체들을 더 이상 new를 통해 생성하지 않는다. 즉, 현재의 코드안에서는 객체를 생성하지 않고 이는 현재 객체가 
직접 작업수행을 하지 않는다는 것을 의미한다. 의존성은 외부에 정의되어 있기 때문에 현재 작업중인 클래스에서는 규격만 맞추면 되고
외부의 다른 객체들에 대해서는 신경을 쓸 필요가 적어진다는 것이다. 다른 클래스의 변화에 신경쓸 필요가 적어진다는 것은 객체간 의존도가
줄어든다는 뜻이고, 이는 결국 IoC의 장점이 된다.  

new를 쓰지 않아 메모리적인 부분에서 강점이 있다고는 하지만 이는 사실 IoC의 중점적인 장점은 아니다. 개발자가 SRP를 지키는 것을 좀 더 쉽게 해줬다는 것이 IoC의 장점일 것이다.
