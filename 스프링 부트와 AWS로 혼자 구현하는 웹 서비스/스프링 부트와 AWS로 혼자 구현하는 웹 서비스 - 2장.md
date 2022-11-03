## 스프링 부트에서 테스트 코드를 작성하자
### 1. 테스트 코드 소개

- **TDD**와 **단위 테스트**는 **다른 이야기**다.

- **TDD**는 **테스트가 주도하는 개발**이고, **테스트 코드를 먼저 작성**한다.
- 반면, **단위 테스트**는 TDD의 첫 번째 단계인 **기능 단위의 테스트 코드를 작성**한다.

![IMG](https://velog.velcdn.com/images/kimtaekjun/post/ac0b5aee-6540-4142-b6c8-d57a1dd2c651/image.gif)

- 순서
1. 항상 실패하는 테스트를 먼저 작성하고(RED)
2. 테스트가 통과하는 프로덕션 코드를 작성하고(Green)
3. 테스트가 통과하면 프로덕션 코드를 리팩토링합니다(Refactor)

#### 1-1. 테스트 코드는 왜 작성해야 할까 ?

- **단위 테스트**는 개발단계 초기에 문제를 발견하게 도와줍니다.
- **단위 테스트**는 개발자가 나중에 코드를 리팩토링하거나 라이브러리 업그레이드 등에서 기존 기능이 올바르게 작동하는지 확인할 수 있습니다.(예, 회귀 테스트)
- **단위 테스트**는 기능에 대한 불확실성을 감소시킬 수 있습니다.
- **단위 테스트**는 시스템에 대한 실제 문서를 제공합니다. 즉, 단위 테스트 자체가 문서로 사용할 수 있습니다.

테스트 코드를 작성하면 더는 사람이 눈으로 검증하지 않게 **자동검증**이 가능하고, **개발자가 만든 기능을 안전하게 보호**해준다.

<br>

### 2. 테스트 코드 작성하기
> Application 클래스는 앞으로 만들 프로젝트의 **메인 클래스**가 된다.

![IMG](https://velog.velcdn.com/images/kimtaekjun/post/ddb18848-adb4-4b68-9539-e06eb006bcb4/image.png)

`@SpringBootApplication` : 스프링 부트의 자동 설정, 스프링 빈 읽기와 생성을 모두 자동으로 설정한다. 특히 **이 위치부터 읽어**가기 때문에 항상 **프로젝트의 최상단**에 위치해야만 한다.  
`SpringApplication.run` : 별도로 외부에 WAS를 두지 않고 애플리케이션을 실행할 때 내부에서 WAS를 실행한다. 항상 서버에 **톰캣을 설치할 필요가 없게 되고**, 스프링 부트로 만들어진 Jar 파일로 실행된다.

내장 WAS를 사용하는 이유는❓
'**언제 어디서나 같은 환경에서 스프링 부트를 배포**'할 수 있기 때문이다. 외장 WAS를 쓴다고 하면 종류와 버전, 설정을 일치시켜야 하고, 새로운 서버를 추가하면 모든 서버에 같은 환경을 구축해야 하기 때문이다.

![IMG](https://velog.velcdn.com/images/kimtaekjun/post/a5ee187f-a3e4-4da2-8f7c-79a0d4bbc1ef/image.png)

HelloController라는 컨트롤러를 생성하였다.

`@RestController` : 컨트롤러를 JSON으로 반환할 수 있게 만들어준다.  
`@GetMapping` : HTTP Method인 Get의 요청을 받을 수 있는 API를 만들어 줍니다.

![IMG](https://velog.velcdn.com/images/kimtaekjun/post/ffa1f5a5-983e-4aeb-8aaf-a77cb41b99f1/image.png)

HelloController 컨트롤러의 테스트 코드를 작성했다.

`@RunWith(SpringRunner.class)` : 스프링 부트 테스트와 JUnit 사이에 연결자 역할을 합니다.  
`@WebMvcTest(controllers = HelloController.class)` : 여러 스프링 테스트 어노테이션 중, Spring Mvc에 집중할 수 있는 어노테이션입니다.  
`@Autowired` : 스프링이 관리하는 빈(Bean)을 주입 받습니다.  
`private MockMvc mockMvc` : 웹 API를 테스트할 때 사용하고, 스프링 MVC 테스트의 시작점입니다.  
`mockMvc.perform(get("/hello"))` : MockMvc를 통해 /hello 주소로 HTTP GET 요청을 합니다.  
`.andExpect(status().isOk())` : HTTP Header의 Status를 검증하고, 흔히 알고 있는 200, 404, 500 등의 상태를 검증합니다.  
`.andExpect(content().string(hello)` : 결과를 검증하고, Controller에서 "hello"를 리턴하기 때문에 이 값이 맞는지 검증합니다.

> 계속 테스트할 때 Execution failed for task ':test'. 라는 에러가 떠, 구글링을 해본 결과
Preferences 에서 빌드 도구 -> Gradle 항목에서 다음을 사용하여 빌드 및 실행, 다음을 사용하여 테스트 실행을 Gradle(디폴드)에서 IntelliJ IDEA로 변경하면 해결이 됩니다.

#### 2-1. 롬복 소개 및 설치