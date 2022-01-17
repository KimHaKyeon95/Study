2022/01/17

## RESTful API
REST API란 REST아키텍처의 제약조건을 준수하는 API(Application Programming Interface)를 뜻한다. 여기서 API란 간단히 말해 
클라이언트가 원하는 어떤 것을 서버에 요청하고 응답을 받아야 할 때 클라이언트와 서버 사이에서 해당 요청을 받고 응답을 서버로 
넘긴다. 이후, 서버로 부터 온 응답을 다시 클라이언트에게 전달하는 역할을 하는 코드를 생각하면 된다. 그렇다면 RESTful API라는 것은 
REST한 API를 말할 것이다. REST란 'Representational State Transfer'의 약자로 자원을 이름으로 구분하여 해당 이름을 가진 자원의 
상태를 주고 받는 모든 것을 의미한다. 여기서 상태는 정보가 될 수 있다. 정보는 XML또는 JSON형태로 전송한다.  
REST는 기본적으로 HTTP프로토콜을 그대로 활용한다. 그렇기 때문에 웹의 장점을 활용할 수 있는 아키텍처이다. REST를 좀 더 
구체적으로 설명하면 HTTP URI를 통해 자원의 이름을 명시하고 HTTP메서드(GET, POST, PUT, DELETE)를 통해 명시한 자원에 대한 
CRUD Operation을 적용하는 것이다. 즉, HTTP메서드를 통해 자원을 해결하도록 설계된 아키텍쳐인 것이다. 이렇게 HTTP표준 프로토콜을 
활용하기 때문에 HTTP 프로토콜을 따르는 모든 곳에서 사용이 가능하다. 또한 이미 인프라가 구축되어있기 때문에 별도의 인프라를 
구축할 필요가 없다. 하지만 표준이라는 것이 따로 존재하지 않고 사용할 수 없다는 메서드가 4가지 뿐이라는 것은 REST API의 단점이 된다.

## RESTful과 MVC
그동안 MVC패턴으로 설계를 진행했다. 이전에 내가 작성했던 MVC패턴에 대한 글은 다음과 같다.
https://github.com/KimHaKyeon95/Study/blob/ab00cbcb72e47fe4e033c400a70d2ac369f281e2/Spring/MVC%20%ED%8C%A8%ED%84%B4.md  
View는 Controller로 부터 출력되는 데이터를 통해 클라이언트로 전송될 내용을 생성하는 코드를 포함하는 HTML문서이다. RestController는 
View를 리턴하는 것이 목적이 아닌 데이터를 전송하는 목적으로 응답이 전송된다. 일단 MVC는 디자인 패턴이고 REST API는 인터페이스이기 때문에 
둘은 공존하지 못하는 상충되는 개념은 아니다. 하지만 개발하는 방식에 차이가 있다면 MVC패턴은 사용자의 요청을 View라는 화면으로 
반환하는 것이고 RESTful API는 사용자의 요청에 대해 JSON 또는 XML형식으로 데이터를 전달하는 것이 주요한 차이일 것이다.
