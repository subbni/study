# Objects.requireNonNull(x)
- Objects 클래스는 `java.util` 패키지에 포함되어 있어 별도의 라이브러리 설치가 필요없이 null 체크가 가능
- 파라미터 값이 null인지 체크하고 만약 그렇다면 `NullPointerException`을 던짐
- 즉, "절대 null이어서는 안 됨"을 명시적으로 표시하고 빠르게 알아채기 위함

# Optional<T>
-  "값이 없을 수도 있다"를 표현해서, 호출자가 존재/부재를 분기 처리하도록 유도한다.

# Objects.requireNonNull과 Optional의 비교
## 공통점
- NullPointerException을 예상치 못 한 곳에서 터뜨리지 않도록 설계된 도구이다.
- 코드에서 "이 값이 null일 수 있는가?"에 대한 의도를 드러내는 역할을 한다.
  - requireNonNull : null이어서는 안 됨 (강제)
  - Optional : 없을 수 있음 (모델링)
## 차이점
- 사용 의미
  - requireNonNull : 방어적 프로그래밍/계약 위반 검출을 위해 사용. null이면 그 즉시 실패시키는 것이 목적
  - Optional : 도메인 모델링/반환 계약 표현을 위해 사용. 없음을 정상 흐름으로 취급하고 호출자가 처리하도록 함
- 발생 시점
  - requireNonNull : null이면 그 자리에서 즉시 예외
  - Optional : null을 값 없음으로 감싸서 이후에 isPresent/map/orElse 등으로 처리
- 적용 위치
  - requireNonNull : 파라미터 검증, 생성자/세터, 필수 의존성 주입 결과 확인 등에 매우 적합
  - Optional : 대표적으로 메서드 반환 타입에 적합함 (필드/파라미터에서 남발하면 오히려 가독성, 비용이 나빠지는 경우 많음)
## 언제, 무엇을 사용할까?
### Objects.requireNonNull
- "여기 들어오는 값은 무조건 있어야 한다"가 명확한 곳
  - ex) 생성자/팩토리에서 필수 필드 검증, public API 파라미터 검증, 외부에서 주입받은 값을 바로 사용해야 할 때
  - 장점 : 실패 지점이 정확하고 디버깅이 쉽다.
  ```java
    this.repo = Objects.requireNonNull(repo, "repo must not be null");
  ```
### Optional
- "없을 수도 있는 결과"를 호출자에게 강제로 처리시키고 싶을 때
  - 조회 결과(있을 수도/없을 수도)
  - 장점 : 호출자가 null 체크를 빼먹기 어렵고, map/filter/orElse로 흐름이 명확해진다.
  ```java
    public Optional<User> findUser(String id) { ... }
  ```
