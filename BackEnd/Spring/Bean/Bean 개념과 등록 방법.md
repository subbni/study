## Bean이란?
- Spring Framework에서 객체를 관리하기 위해 사용하는 개념으로, **스프링 컨테이너가 관리하는 객체**를 뜻한다.
- 주로 Service, Repository, Controller, Configuration 등 모든 객체가 Bean으로 관리될 수 있다.
## Bean 등록 방법
크게 3가지가 있다.
#### 1. XML 설정 (구버전 방식)
```xml
<bean id="userService" class="com.example.UserService"/>
```
- 과거에는 XML 설정으로 Bean을 등록했으나, 최근에는 거의 사용하지 않는다.
#### 2. @Configuration + @Bean
- `@Configuration` 클래스 내에서 `@Bean` 메서드를 선언하면 메서드가 반환하는 객체가 빈으로 등록된다.
```java
@Configuration
public class AppConfig {
    @Bean
    public UserService userService() {
        return new UserService();
    }
}
```
#### 3. Component Scan
- 클래스에 `@Component`, `@Service`, `@Repository`, `@Controller` 등을 붙여 자동으로 Bean으로 등록되게 한다.
- 스프링이 컴포넌트 스캔을 통해 어노테이션이 붙은 클래스를 찾아 등록한다.
```java
@Component
public class UserService {
    // ...
}

```
