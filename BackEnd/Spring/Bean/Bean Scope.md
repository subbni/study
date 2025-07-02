## Bean Scope
Bean이 어떤 범위로 관리되는지를 Scope로 지정할 수 있다.

#### 1. Singleton (기본값)
- Spring Container 당 Bean 인스턴스 단 1개만 생성
- 모든 요청에서 같은 객체를 공유한다.
- Controller, Service, Repository 등이 주로 이 스코프를 사용한다.
> 여기서 '요청'은 getBean() 호출이나 의존성 주입 시점을 의미한다.

#### 2. Prototype
- Bean을 요청할 때마다 새로운 객체를 생성
- 스프링이 생성까지만 관리하며, 이후 소멸은 개발자가 관리하여야 한다.
- 상태를 갖는 객체를 사용하거나 매번 다른 동작이 필요할 때 사용한다.

#### 3. Request (웹 전용)
- HTTP 요청마다 새로운 Bean 생성
- 요청이 끝나면 Bean이 소멸된다.
- 웹 어플리케이션에서만 사용 가능하다.

#### 4. Session (웹 전용)
- HTTP 세션마다 하나의 Bean 생성
- 로그인 사용자별로 개별 Bean을 유지할 수 있다.
  - ex) 사용자별 장바구니, 설정 캐시 등

#### 5. Application (웹 전용)
- ServletContext 범위에서 Bean 하나만 생성된다.
- 어플리케이션 전체에서 공유되는 전역 Bean
> **ServletContext가 뭔데?**
>
> 웹 어플리케이션을 배포하면, 톰캣 같은 서블릿 컨테이너가 서블릿 컨텍스트(Servlet Context)라는 공간을 하나 만든다.
> 이 공간은 웹 어플리케이션 전체에서 공유되는 저장소이다.

#### 6. Websocket (웹 전용)
- 웹소켓 연결마다 개별 Bean 생성
- 실시간 연결마다 독립된 객체가 필요할 때 사용한다.
