어떤 순서로 수행될까?

#### 1. Spring Boot 어플리케이션 실행
```java
@SpringBootApplication
public class MyApplication {
	public static void main(String[] args) {
		SpringApplication.run(MyApplication.class, args);
	}
}
```
- SpringApplication.run()이 실행되면, 내부적으로 `ApplicationContext`를 생성한다.
- `ApplicationContext`는 Spring의 핵심 컨테이너로, 등록된 객체(Bean)을 담아두고 관리하는 역할을 한다.
#### 2. 빈 정의(Bean Definition) 등록
- `@ComponentScan`을 통해
  - 현재 패키지와 하위 패키지를 스캔하여 빈으로 등록하기 위한 어노테이션이 붙어있는 클래스를 찾아 빈으로 등록한다.
  - ex) `@Component`, `@Service`, `@Controller` 등 ...
- `@Configuration`이 달린 클래스에서
  - `@Bean` 어노테이션이 붙은 메서드를 찾아 실행 후 리턴된 객체를 빈으로 등록한다.
- `@EnableAutoConfiguration`에 의해 외부 라이브러리에서 제공하는 빈도 자동으로 등록된다.
  - ex) spring-boot-starter 의존성의 빈들이 자동 등록됨
#### 3. 의존성 분석 (DI 준비)
- 각 빈의 생성자, 필드, 세터 등을 분석하여 어떤 의존성이 필요한지 파악한다.
  - 어떤 타입의 빈을 필요로 하는지
  - `@Qualifier` 확인
  - `@Primary` 빈 확인
  - 순환참조가 발생하지 않는지
#### 4. 의존성 주입 (DI 실행)
- ApplicationContext가 각 Bean의 인스턴스를 생성한다.
- 주입 방식에 따라 의존성을 주입한다. (생성자/필드/세터)
- 빈이 생성되고 의존성이 주입된 뒤, 초기화 메서드(ex. `@PostConstruct`)가 호출된다.
