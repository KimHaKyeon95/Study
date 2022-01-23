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

그렇다면 Servlet과 Spring의 관계는 어떻게 되는 것일까?

## Servlet과 Spring
처음 Spring Boot부터 공부를 시작했을 때는 Spring과 서블릿은 전혀 관계없는 기술인줄 알았다. 하지만 서블릿을 기반으로 Spring이 돌아간다는 
것을 알게 되고 적잖은 충격을 받았던 것 같다. 스프링 부트는 스프링을 좀 더 편하게 사용하기 위해 제작된 서브 프로젝트이므로 스프링에서 
서블릿이 어떻게 동작하는지 알게되면 이를 앞으로의 공부에 반영할 수 있을 것 같았다.  
Spring MVC내부에서는 DispatcherServlet이라는 컨트롤러가 존재한다. 이것의 역할은 Servlet Container로부터 들어오는 요청을 
관제하는 것이다. 덕분에 우리는 여러개의 서블릿을 만들 필요가 없다. 하나의 서블릿에서 클라이언트의 모든 요청을 받고 이 서블릿에서 
우리가 설정한 컨트롤러로 URL요청을 넘겨주는 것이다.
