2022/01/18

# Spring Boot의 장점

## AutoConfiguration
스프링 레거시에 비해서 간단한 점을 생각했을 때 가장 먼저 떠오른 부분이다. 스프링 레거시에서는 매번 Bean을 등록해줘야했다. 
@Controller, @Service, @Repository, @Configuration 같은 설정들을 
```xml
	<context:component-scan base-package="com.my.product.dao"></context:component-scan>
	<context:component-scan base-package="com.my.customer.dao"></context:component-scan>
	<context:component-scan base-package="com.my.order.dao"></context:component-scan>
```
이렇게 servlet-context.xml파일에 매번 등록해야 했는데 Spring Boot에서는 이럴 필요가 없어졌다. 하지만 모든 빈이 다 등록되지는 않는다. 
이는 기본 패키지명을 따라가는 스프링 부트의 특징 때문에 그러한데, 다른 패키지명을 가진 Bean을 등록하려면 @ComponentScan을 
사용하면 되지만 이는 그닥 권장되는 내용은 아니며, 프로젝트를 진행할 때는 기본 패키지명은 지키는 것이 바람직하다.
