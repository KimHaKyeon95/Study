2022/01/09

### MVC 패턴
MVC는 Model-Veiw-Controller의 약자이다. MVC패턴은 디자인 패턴 중 하나인데, 여기서 **디자인 패턴**이란 어떤 것을 
개발하는 중에 발생한 문제점을 정리해 상황에 따라 간편하게 적용하여 쓸 수 있는 것을 정리하여 특정한 규약을 통해 쉽게 쓸 수 있도록
형태를 만든 것을 의미한다. MVC패턴은 하나의 애플리케이션을 구성할 때 해당 구성요소를 Model, View, Controller로 구분한 패턴이다.  

각각의 역할은 다음과 같다.
* Model은 데이터 디자인을 담당한다. Controller에서 받은 데이터를 저장하는 역할을 한다.
* View는 서버의 데이터를 렌더링하여 보여주는 역할을 한다. html은 정확히는 Viewer라고 할 수 없다. 서버의 응답을 출력해주지 않기 떄문이다.
* Controller는 클라이언트의 요청(URL에 따른 요청)을 받고 이에 대한 응답을 주는 로직을 담당한다.  

Spring에서는 이런 MVC패턴을 구현하기 위한 모듈을 제공해준다.

### Spring MVC Framework
![image](https://user-images.githubusercontent.com/81364044/148770911-0619beed-d83a-4e6f-a418-db15855b70bb.png)
동작 순서는 다음과 같다.
1. Client로 부터 요청이 들어오면 DisapatcherServlet이 호출.
2. DispatcherServlet은 HandlerMapping으로 요청을 던짐. URL을 분석하여 적합한 Controller를 선택해 반환.
3. Controller는 비즈니스 로직을 처리하고 해당 결과를 Model에 저장함. 기존의 Servlet에서는 HttpServletRequest를 
사용하여 정보를 전달하고 이런 Request를 View에서도 사용했지만 Spring에서는 Model을 사용함. 이렇게 함으로써 응답할 데이터 Model과 응답할 레이아웃View를 완벽히 구분할 수 있다. View Name을 DispatcherServlet에 반환. 
Controller는 ViewName을 String으로 반환하는데 스프링 컨테이너가 이런 String값을 알아서 ModelAndView타입으로 만들어 
DispatcherServlet에서 반환함.
4. DispatcherServlet은 ViewResolver를 호출하고 반환된 ViewName에 해당하는 View를 찾고 결과를 넘겨 보여주도록함.
5. View는 Model객체에서 화면 표시에 필요한 객체를 찾고 화면에 표시해줌.
