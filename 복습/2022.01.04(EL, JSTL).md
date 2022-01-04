2022/01/04(EL, JSTL)
=============
   
   
 ## JSP Action
JSP의 기본 문법 중 하나인 <% %>은 expression이라고 한다. expression은 다음과 같이 사용한다.  
```jsp
<% 
	List<String> str = (List)request.getAttribute("list"); 
	if(list == null){
		list = new ArrayList();
		request.setAttribute("list", list)
	}
%>
```
위와 같은 코드를 많이 사용한다면 코드의 복잡도가 높아져 가독성이 떨어지게 된다.  
  
이를 Action Tag인 <jsp:useBean>을 사용하면 다음과 같이 나타낼 수 있다.
```jsp
<jsp:useBean id = "list" class="java.util.ArrayList" scope="request"/>
```
각 요소에 대해 하나씩 알아보면 
  > id는 Attribute의 이름과 해당 Attribute의 값이 들어갈 객체나 변수의 이름을 나타낸다.  
  > 
  > scope는 저장공간을 나타낸다. request는 ServletRequest를 나타낸다.
  > 
  > class는 자바객체를 생성할 때 사용하는 클래스 이름을 뜻한다.
  > 
  > 이외에도 type이라는 속성을 통해 참조변수를 선언할 때 사용할 타입을 지정할 수 있다.  

이렇게 JSP Action을 사용하면 긴 코드를 짧게 줄일 수 있어 가독성을 높일 수 있지만 다음과 같은 상황이 발생할 수 있다.
```jsp
<%= orderInfo.getOrderNo() %>
<jsp:getProperty name="orderInfo" property="orderNo"/>

<%= orderInfo.getOrderCustomer().getId() %> //Order내부의 Customer객체의 id
<jsp:getProperty name="orderInfo" property=?????/>
```
Order라는 객체에는 Customer가 포함된다. Has a관계로 인해 객체에서 내부의 다른 객체를 출력해야할 때는 위와 같이 JSP Action을 사용해서는 한계가 있다.  
이러한 단점은 **EL**을 통해 보완할 수 있다.  

## EL(Expression Language)
EL은 ${}을 통해 표현한다.
```jsp
<%--Expression Language를 활용(has a 관계의 property를 표현 --%>
<%= orderInfo.getOrderCustomer().getId() %> 은 밑의 코드와 같다.
${requestScope.orderInfo.orderCustomer.id}
```
위와 같이 has a 관계의 property를 더 간단하고 효과적으로 표현할 수 있기 때문에 일반 JSP또는 JSP Action보다는 EL을 쓰는 것이 권장된다.
#### EL의 연산
EL을 사용해서 연산 결과를 출력할 수 있다.  
![image](https://user-images.githubusercontent.com/81364044/148038886-e3a3f692-4b58-4863-bfc4-2444db6cf8dc.png)  
이 부분에서 주목할 점은 3/4인데, 일반적으로 Java에서는 정수와 정수를 나누면 정수가 출력되지만 EL에서는 결과가 실수로 출력된다.  

비교연산은 다음과 같다.  
![image](https://user-images.githubusercontent.com/81364044/148039116-d5956160-9591-4f89-b0ef-f4c8e541f34d.png)  

객체는 다음 키워드들을 통해 출력된다.
![image](https://user-images.githubusercontent.com/81364044/148039386-e232d2ed-03c0-442e-8a70-06f14da0221f.png)  
여기서 applicationScope는 Servlet Scope에 저장된 객체를 의미한다.  

위와 같이 EL을 통해 객체의 Property를 더 쉽게 출력할 수 있지만 반복문, 조건문 등은 EL로 처리할 수 없다. 이는 **JSTL**을 사용하여 해결할 수 있다.

## JSTL(Java Standard Tag Library)
JSTL은 EL을 기반으로 하며 JSTL용 라이브러리를 설치하여 사용할 수 있다. 경로는 일반적인 Dynamic Web Project의 경우 WEB-INF\lib에 설치하면 된다.  

#### 변수의 선언
    <c:set var="opt" value="${param}"/>
해당 코드는 다음과 같은 기능을 한다.  
```
String opt = request.getParameter("opt")
```
여기서 opt는 파라미터로부터 얻는 변수를 뜻한다.

#### 조건문(if, else if)
위는 jsp로 작성을 했을 때, 아래는 JSTL로 작성을 했을 때이다. 두 코드는 같은 실행결과를 보인다.
```jsp
<%
if(opt == null || opt.equals("")){
  out.print("작업을 선택하세요");
}
 %>
 
<c:if test="${empty opt}">작업을 선택하세요</c:if>
```

 ```jsp
 <%
if(opt == null || opt.equals("")){
  out.print("작업을 선택하세요");
}
 %>
<c:if test="${empty opt}">작업을 선택하세요</c:if>
 ```
```jsp
 <%
if(opt.equals("add")){
}else if(opt.equals("list")){
}else if(opt.equals("modify")){
else if(opt.equals("remove")){
}
 %>
 
 <c:choose>
 	<c:when test="${opt == 'add' }">추가작업을 선택했습니다.</c:when>
 	<c:when test="${opt == 'list' }">목록작업을 선택했습니다</c:when>
 	<c:when test="${opt == 'modify' }">수정작업을 선택했습니다</c:when>
 	<c:when test="${opt == 'remove' }">삭제작업을 선택했습니다</c:when>
 	<c:otherwise>작업을 선택해주세요</c:otherwise>
 </c:choose>
```
<c:otherwise></c:otherwise>는 else의 역할을 한다.  

List<Customer>의 참조형 변수인 list가 존재한다고 할 때, 이를 조회하여 출력하는 코드는 다음과 같다.  
````jsp
 <%
 List<Customer> list = (List)request.getAttribute("list");
 for(Customer c:list){
 	out.print(c.getId());
 } %>
 <c:set var="list" value="${requestScope.list }"/>
 <table>
 <tr><th>번호(index:coount)</th><th>아이디</th><th>이름</th></tr>
	 <c:forEach items="${list}" var="c" varStatus="status">
	 <tr>
	  <td>${status.index}:${status.count}</td><td>${c.id}</td><td>${c.name}</td>
	 </tr>
 </c:forEach>
 </table>
````
위와 같은 코드의 출력은 다음과 같다.  
![image](https://user-images.githubusercontent.com/81364044/148053528-c45b8b7a-a2b4-4577-9924-7b72a0a2f203.png)  
forEach문을 사용한 것과 같은 효과를 보이며 태그를 통해 일정한 형식의 리스트로 출력되었다.
