# DI
`의존성 주입(Dependency Injection)` : 필요한 객체(의존성)을 외부에서 주입받는 방식
<img width="715" alt="Screenshot 2025-04-11 at 00 11 29" src="https://github.com/user-attachments/assets/a2101e9b-a808-4de3-bd03-17de8f523d2e" />

## ▶︎ DI 적용 전 기존 방식
DI의 개념이 없기 전에는, 객체를 사용하기 위해 어떤 방식을 사용했을까?

### 1. 직접 객체 생성
- 어떤 객체 내에서 사용하려는(의존하려는) 객체를 `new` 생성자를 통해 직접 생성한다.
```java
public class CafeController {
	private StarbucksService starbucksService = new StarbucksService();
	
	public void orderCoffee() {
		starbucksService.brew();
	}
}
```
- 문제점 : CafeController와 StarbucksService가 강하게 결합되어 있다.
  - 만일 StarbucksService가 아니라 다른 서비스로 대체하고 싶다면 CafeController의 코드를 직접적으로 건드려야 한다.

### 2. 팩토리 패턴
- 객체의 생성을 전담하는 팩토리 클래스를 사용하는 방법이다.
```java
public interface CoffeeService {
	void brew();
}

public class StarbucksService implements CoffeeService {
	public void brew() {
		System.out.println("Starbucks 커피 제조 중...");
	}
}

public class CoffeeServiceFactory { // 팩토리 클래스
	public static CoffeeService createCoffeeService() {
		return new StarbucksService();
	}
}

public class CafeController {
	private CoffeeService coffeeService;

	public CafeController() {
		this.coffeeService = CoffeeServiceFactory.createCoffeeService(); // 팩토리 클래스 사용
	}

	public void orderCoffee() {
		coffeeService.brew();
	}
}
```
- 인터페이스 + 팩토리 클래스 활용으로, 이제 CafeController에서 Starbucks가 아닌 다른 서비스를 이용해야 하는 경우에도 CafeController 코드를 건드릴 필요 없이 CoffeeServiceFactory 안의 코드만 변경해주면 된다.

#### 팩토리 패턴과 DI
- 팩토리 패턴은 객체 생성의 책임을 분리한다는 점에서 DI와 비슷한 개념을 공유한다.
- 하지만 **객체를 사용하는 쪽에서 직접 팩토리를 호출하여 객체를 받아오므로, 스스로 가져다쓰는(Pull) 방식**에 해당한다.
- 반면 **DI는 외부에서 객체를 주입해주는(Push) 방식**이다.

## ▶︎ DI (Dependency Injection)
- 객체를 직접 생성하지 않고, 필요한 객체를 외부에서 주입해준다.
- 객체의 생성, 주입, 생명주기 관리를 외부가 담당하게 된다.
```java
// 인터페이스
public interface CoffeeService {
	void brew();
}

// 구현체
public class StarbucksService implements CoffeeService {
	public void brew() {
		System.out.println("Starbucks 커피 제조 중...");
	}
}

// 컨트롤러 (서비스를 주입받음)
public class CafeController {
	private final CoffeeService coffeeService;

	// 생성자 주입
	public CafeController(CoffeeService coffeeService) {
		this.coffeeService = coffeeService;
	}

	public void orderCoffee() {
		coffeeService.brew();
	}
}

// 실행 메인 클래스 : 의존성 주입 및 애플리케이션 메인 클래스
public class Main {
	public static void main(String[] args) {
		
		// 한 번만 생성
		CoffeeService coffeeService = new StarbucksService();
		
		// 생성자로 주입
		CafeController cafeController = new CafeController(coffeeService);
		RestaurantController restaurantController = new RestaurantController(coffeeService);

		// 각 컨트롤러에서 동일한 CoffeeService 사용
		cafeController.orderCoffee();
		restaurantController.orderCoffee();
	}
}
```
### 장점
1. 객체 재사용
   - 필요한 객체를 한 번만 생성하고 여러 곳에 주입해서 사용하므로, 리소스 낭비를 최소화한다.
2. 유지보수성 향상
   - 객체 생성은 외부에서 담당하여 해당 객체를 사용하는 클래스내 코드 변화를 최소화한다.

위 예제에서는 개발자가 작성한 `Main` 클래스가 의존성 주입을 담당하고 있다.
Spring을 사용하면 이 역할을 **Spring Container가 대신하는데,** 이를 나타내는 개념이 바로 `IoC(Inversion of Control)`, **제어의 역전**이다.

---

# Spring IoC
`제어의 역전` : 객체의 생성, 관리, 의존성 주입의 책임이 개발자에서 Spring 프레임워크로 역전된 것을 의미한다.
> **왜 제어의 역전이라고 할까?**
> 
> 기존에는 개발자가 new 키워드로 직접 객체를 생성하여 필요한 클래스에 주입하였다. (객체 안에서든, 메인 메서드 안에서든)
> 그러나 스프링 프레임워크에서는 스프링 컨테이너가 알아서 객체를 생성하고, 필요한 곳에 주입(DI)함으로써 객체 제어권이 개발자에서 프레임워크로 역전됨을 의미한다.

### 👀 IoC와 DI는 어떤 관계인가?
- IoC의 범위(객체 생성, 관리, 의존성 주입)에 DI(의존성 주입)가 포함된다.
