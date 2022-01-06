### 두 Injection의 차이
우선 사용하는 방법부터 다르다. Setter Injection은 다음과 같이 수행할 수 있다.
```java
public class Product {
    private String prodNo;
    private String prodName;
    private int prodPrice;
    ...
    public void setProdNo(String prodNo) {
      this.prodNo = prodNo;
    }
    public String getProdName() {
      return prodName;
    }
    public void setProdName(String prodName) {
      this.prodName = prodName;
    }
    public int getProdPrice() {
      return prodPrice;
    }
    public void setProdPrice(int prodPrice) {
      this.prodPrice = prodPrice;
    }
  }
  ```

```xml
<bean id="p"  class="com.my.product.vo.Product">
	   <property name="prodNo"      value="C0005"/>
	   <property name="prodName">
	   	<null/>
	   </property>
	   <property name="prodPrice"   value="1000"  />
	</bean>
```
  각 Setter를 Injection해주었다.  
  
  Constructor Injection은 다음과 같이 수행한다.
  ```xml
<bean id="pp" class="com.my.product.vo.Product" >
		<constructor-arg index="0" value="C0007"/>
		<constructor-arg index="1" value="커피7"/>
		<constructor-arg index="2" value="1500"/>
	</bean>
```

두 Injection에는 차이점이 존재하는데 우선 Setter의 주입은 객체가 메모리에 적재된 후에 Bean을 주입하고, Constructor주입은 
객체를 생성자로 생성하는 시점에 주입을 한다는 점이다. Setter의 주입은 문제가 되지 않지만 Constructor의 경우는 순환참조가 
문제가 될 수 있다.  
두 객체가 생성될 때 서로를 필요로 하면 Setter의 경우 객체를 이미 적재한 상태라 서로를 필요로 하는 상황에서도 
문제가 없지만 Constructor주입은 메모리에 적재가 되지 않은 상태에서 서로의 객체를 필요로 하기 때문에 문제가 발생한다.  
이러한 특징때문에 Constructor Injection은 순환 의존성을 파악해 디자인 패턴이 어떤 상태인지 파악할 수 있다.
