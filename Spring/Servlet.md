2022/01/23

Servlet을 공부하고 Spring, Spring Boot로 넘어오게 되었다. Servlet에 대한 개념을 정리해본 적은 없어서 가장 기본이 되는 Servlet에 
대해 이 기회에 정리를 하려 한다.

## Servlet
우선 간단하게 서블릿이라는 개념에 대해서 얘기하자면, 서블릿이란 자바를 사용하여 웹 페이지를 동적으로 생성하는 프로그램, 또는 그 사양을 
말한다. Servlet Container에 의해 관리, 실행된다. 일반적으로 서블릿이 동작하는 과정은 다음과 같다.
1. 클라이언트가 URL을 Http Request를 보내면 서블릿 컨테이너가 해당 요청을 바탕으로 HttpServletRequest와 HttpServletResponse객체를 
생성한다.
2. 서버에 정보를 제공하는 web.xml파일은 클라이언트가 요청한 URL이 어떤 서블릿에 대한 요청인지 확인한다. 
3. 해당 서블렛에서 service메서드를 호출하고 클라이언트의 요청종류(POST, GET)에 따라 해당 메서드를 호출한다.
4. doGet, doPost메서드가 동적 페이지를 생성하고 HttpServletResponse객체에 응답을 보낸다.
5. 응답이 끝나면 HttpServletRequest, HttpServletResponse객체는 소멸된다.

## Servlet LifeCycle
Tomcat은 자바 서블릿을 이용하여 데이터 요청에 대한 응답을 자바코드로 처리하고 응답을 클라이언트에게 보내주는 웹서버이다.
이러한 톰캣이 실행되면 ServletContext라는 객체가 생성된다. 해당 객체에는 현재 배포한 웹 컨텍스트(호출, 응답을 위한 환경설정을 해둔 정보의 총칭)의 경로를 나타낸 realPath와 버전정보인 major v, minor v가 정의되어있다. ServletContext객체는 톰캣서버가 켜지면 생성되고 서버가 꺼질 때 사라진다. 가장 오랜 시간동안 생성시간을 유지한다.
이제 웹에서 URL호출을 하게될 것이다. 예를 들어 firstEE라는 웹프로젝트와 /first라는 URL로 매핑된 어떤 서블릿이 존재한다고 가정하자.  
web.xml에는 다음과 같이 설정되어있다.
![image](https://user-images.githubusercontent.com/81364044/150684613-16c7b8d6-cee8-4575-98ea-0c2b1697794f.png)
servlet-mapping에서 /first인 URL을 찾고 이와 연결되는 서블릿의 이름을 찾는다. 그리고 서블릿이름을 통해 해당 클래스파일을 찾는다.  
만약 해당 클래스의 객체가 존재하지 않는다면 JVM메모리에 클래스를 로드하고 기본 생성자를 호출한다. 객체는 호출시마다 생성되는 것이 아니라 호출시에 한번만 생성된다. 클래스가 로드되면 init()이라는 메서드를 호출한다.  
init()이라는 메서드는 서블릿을 초기화 하기 위해서 ServletConfig라는 파라미터를 갖게 되고 해당 config로 초기화를 한 뒤에 파라미터가 없는 init()메서드를 호출한다. 서블릿의 역할에 따라 서블릿 객체가 생성되는 시점에 필요한 행동들을 init()을 오버라이딩하여 구현하게 된다. 이 때 웹 어플리케이션이 실행되는 시점에 init()을 실행하고 싶다면 <load-on-startup>태그를 사용하여 이를 자동으로 실행되게 할 수 있다.
```xml
  <servlet>
        <servlet-name>FirstServlet</servlet-name>
        <servlet-class>com.my.first.FirstServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
     </servlet>
```
태그 내부의 숫자는 우선 순위를 뜻하며 0에 가까울 수록 먼저 실행된다.  
위의 상황은 해당 서블릿 객체가 생성되어 있지 않은 상태였고 이미 해당 과정을 거쳐 생성된 서블릿 객체가 있으면 service()메서드를 호출하게 된다. service()메서드는 doPost()와 doGet()로 클라이언가 요청한 메서드방식에 따라 선택할 수 있다. 이렇게 service()메서드가 호출되기 전에 HttpServletRequest와 HttpServletResponse객체가 생성된다.  
  위에서 정의한 FirstServlet은 HttpServlet을 상속받았다. init()메서드가 실행되면 HttpServlet의 필드인 sc는 ServletContext객체를 가리키게 될 것이다. 위에서 만들어진 두 객체인 HttpServletRequest와 HttpServletResponse객체는 doGet()과 doPost()의 파라미터로 들어가게 된다. doGet()과 doPost()메서드가 실행되고 응답이 오면 두 객체는 소멸된다.  
 그리고 destroy()메서드를 통해 서블릿 객체를 제거한다.  
  
  
그렇다면 Servlet과 Spring의 관계는 어떻게 되는 것일까?

## Servlet과 Spring
처음 Spring Boot부터 공부를 시작했을 때는 Spring과 서블릿은 전혀 관계없는 기술인줄 알았다. 하지만 서블릿을 기반으로 Spring이 돌아간다는 
것을 알게 되고 적잖은 충격을 받았던 것 같다. 스프링 부트는 스프링을 좀 더 편하게 사용하기 위해 제작된 서브 프로젝트이므로 스프링에서 
서블릿이 어떻게 동작하는지 알게되면 이를 앞으로의 공부에 반영할 수 있을 것 같았다.  
Spring MVC내부에서는 DispatcherServlet이라는 컨트롤러가 존재한다. 이것의 역할은 Servlet Container로부터 들어오는 요청을 
관제하는 것이다. 덕분에 우리는 여러개의 서블릿을 만들 필요가 없다. 하나의 서블릿에서 클라이언트의 모든 요청을 받고 이 서블릿에서 
우리가 설정한 컨트롤러로 URL요청을 넘겨주는 것이다.
