2022.01.06
Spring에 대해 공부하던 중 강의에서 독특한 부분이 나왔다.
```java
	@GetMapping("productadd")
	public ModelAndView add(String prodNo, String prodName, int prodPrice) {
		logger.warn(prodNo + ":" + prodName + ":" + prodPrice);
		Product p = new Product(prodNo, prodName, prodPrice);
		return null;
	}
 ```
 ```java
 	@GetMapping("productadd")
	public ModelAndView add(Product p) { 
		logger.warn(p);
		return null;
	}
 ```
 위의 두가지는 같은 WARN로그를 출력한다. 얼핏보면 당연하다고 넘어갈 수 있지만 Reflection이라는 개념을 생각하면 쉽다고 하니 한번 Reflection을 
 알아봤다.
### Reflection
 리플렉션은 '구체적인 클래스 타입을 알지 못해도, 그 클래스의 메서드, 타입, 변수들을 접근할 수 있도록 해주는 API'라고 한다. 
 Spring, Android에서도 해당 개념이 사용된다. Spring에서는 BeanFactory라는 Spring Container가 호출될 때의 객체의 인스턴스를 
 생성하게 되는데 이 때 필요한 기술이 리플랙션이다.
