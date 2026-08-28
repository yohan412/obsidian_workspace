
### Q. Spring DI/IoC는 어떻게 동작하나요?

**[원본 답변]** IoC(제어의 역전)은 프로그램의 제어 흐름을 직접 제어하는 것이 아니라 외부에서 관리하는 것으로 코드의 최종호출은 개발자가 제어하는 것이 아닌 프레임워크의 내부에서 결정된 대로 이루어집니다.

DI(의존관계 주입)은 Spring 프레임워크에서 지원하는 IoC의 형태로 클래스 사이의 의존관계를 빈 설정 정보를 바탕으로 컨테이너가 자동으로 연결해줍니다.

스프링에서는 스프링 컨테이너 ApplicationContext를 이용하여 설정 정보를 생성, 등록하고 필요한 객체를 생성자 혹은 setter를 통해 주입합니다.

### Q. Spring Bean이란 무엇인가요?

**[원본 답변]** IoC 컨테이너 안에 들어있는 객체로 필요할 때 IoC컨테이너에서 가져와서 사용합니다. @Bean 을 사용하거나 xml설정을 통해 일반 객체를 Bean으로 등록할 수 있습니다.

### Q. 스프링 Bean의 생성 과정을 설명해주세요.

**[원본 답변]** 객체 생성 → 의존 설정 → 초기화 → 사용 → 소멸 과정의 생명주기를 가지고 있습니다. Bean은 스프링 컨테이너에 의해 생명주기를 관리하며 빈 초기화방법은 @PostConstruct 를 빈 소멸에서는 @PreDestroy 를 사용합니다.

생성한 스프링 빈을 등록할 때는 ComponentScan을 이용하거나 @Configuration 의 @Bean 을 사용하여 빈 설정파일에 직접 빈을 등록할 수 있습니다.

### Q. 스프링 Bean의 Scope에 대해서 설명해주세요.

**[원본 답변]** 빈 스코프는 빈이 존재할 수 있는 범위를 뜻하며 싱글톤, 프로토타입, request, session, application 등이 있습니다.

싱글톤은 기본 스코프로 스프링 컨테이너의 시작과 종료까지 유지되는 가장 넓은 범위의 스코프입니다.

프로토타입은 빈의 생성과 의존관계 주입까지만 관여하고 더는 관리하지 않는 매우 짧은 범위의 스코프입니다.

request는 웹 요청이 들어오고 나갈때까지 유지하는 스코프, session은 웹 세션이 생성, 종료할때까지, application은 웹 서블릿 컨텍스트와 같은 범위로 유지하는 스코프입니다.

### Q. IoC 컨테이너의 역할은 무엇이 있을까요?

**[원본 답변]**애플리케이션 실행시점에 빈 오브젝트를 인스턴스화하고 DI 한 후에 최초로 애플리케이션을 기동할 빈 하나를 제공해준다

### Q. DI 종류는 어떤것이 있고, 이들의 차이는 무엇인가요?

**[원본 답변]** DI는 세가지 방법이 있습니다. 생성자 삽입, Setter를 이용한 메소드 매개 변수 삽입, 필드 주입이 있습니다.

생성자 주입은 생성자 호출시점에 딱 1번만 호출되는 것을 보장하며 불변, 필수 의존관계에 사용합니다.

Setter주입은 선택, 변경 가능성이 있는 의존관계에 사용되며 스프링빈을 선택적으로 등록이 가능합니다.

필드 주입은 `@Autowired` 를 사용하는데 외부에서 변경이 불가능하여 테스트 하기 힘듭니다. DI 프레임워크 없이는 작동하기 힘들며, 주로 애플리케이션과 관계없는 테스트코드나 `@Configuration` 같은 스프링 설정 목적으로 사용합니다.

### Q. Autowiring 과정에 대해서 설명해주세요.

**[원본 답변]** 컨테이너에서 타입(인터페이스 또는 오브젝트)을 이용해 의존 대상 객체를 검색하고 할당할 수 있는 빈 객체를 찾아 주입한다

### Q. Spring Web MVC의 Dispatcher Servlet의 동작 원리에 대해서 간단히 설명해주세요.

**[원본 답변]**_(원문 답변 없음)_

- **추가 설명:**
    
    1. 클라이언트의 모든 HTTP 요청을 중앙 프론트 컨트롤러인 `DispatcherServlet`이 가장 먼저 수신합니다.
        
    2. `HandlerMapping`을 통해 요청 URI에 매핑된 적절한 Controller(Handler)를 찾습니다.
        
    3. `HandlerAdapter`를 통해 해당 Controller의 메서드를 실행합니다.
        
    4. Controller가 비즈니스 로직을 처리한 뒤 반환한 View 이름이나 데이터를 기반으로, `ViewResolver`를 통해 적절한 View를 생성하거나 `@ResponseBody`를 통해 JSON/XML 형태의 응답 본문을 완성하여 클라이언트에 돌려줍니다.
        

### Q. 프론트 컨트롤러 패턴이란 무엇인가요?

**[원본 답변]** 클라이언트의 다양한 요청마다 서블릿을 만들어서 사용한다고 하면 개발과 유지보수의 효율이 떨어질 수 밖에 없습니다. 프론트 컨트롤러 패턴을 사용함으로써 각 요청을 적절한 곳으로 위임해줌으로써 개발과 유지보수의 효율성이 증가하고 모든 요청에 대해 보안, 국제화, 라우팅 및 로그와 같은 일반적인 기능을 한 곳에서 캡슐화할 수 있습니다. Spring에서는 DispatcherServlet이 프론트 컨트롤러 패턴을 사용한 예이며, DispatcherServlet이 Bean으로 등록되어 package를 scan하고 @Controller, @RestController 애노테이션을 확인하여 어떠한 요청이 들어왔을 때 적절한 Handler Method에 위임해줍니다.

### Q. Servlet Filter와 Spring Interceptor의 차이는 무엇인가요?

**[원본 답변]** Filter는 Servlet Filter로써 javax.servlet 스펙에 포함되는 클래스입니다.

Interceptor는 Spring MVC 스펙에 포함되어 있는 클래스입니다.

Filter는 Servlet에서 전후처리를 담당하며, Interceptor는 Spring에서 Handler를 실행하기 전후나, ViewResolver를 통해 컨트롤러에서 리턴한 View Name으로부터 렌더링을 담당할 View 오브젝트를 준비해 돌려준 후 실제 View를 렌더링한 후에 어떠한 처리를 담당합니다.

Filter는 Web Application(Tomcat을 사용할 경우 web.xml)에 등록하며, Interceptor는 Spring의 Application Context에 등록합니다.

Filter는 Method Signature에 있는 Argument인 HttpServletRequest 혹은 HttpServeltResponse를 ServletRequest, ServletResponse 등으로 교체할 때 사용하거나, 데이터 변환(다운로드 파일의 압축 및 데이터 암호화 등), XSL/T를 이용한 XML 문서 변경, 사용자 인증, 자원 접근에 대한 로깅 등에 사용합니다.

Interceptor의 경우 AOP를 흉내내거나, Spring 애플리케이션에서 전역적으로 전후처리 로직에서 예외를 사용하도록 하거나, Handler Method에서 사용자의 권한을 체크해서 다른 동작을 시켜준다거나 할 때 사용합니다.

### Q. Spring에서 CORS 에러를 해결하기 위한 방법을 설명해주세요.

**[원본 답변]**Servlet Filter를 사용하여 커스텀한 Cors 설정하거나, WebMvcConfiguer를 구현한 Configuration 클래스를 만들어서 addCorsMappings()를 재정의할 수도 있고, 마지막으로 Spring Security에서 CorsConfigurationSource를 Bean으로 등록하고 config에 추가해줌으로써 해결할 수 있습니다.

Controller 클래스에 @Crossorigin 어노테이션을 통해 해결할 수 있습니다.

### Q. Bean/Component 어노테이션에 대해서 설명해주시고, 둘의 차이점에 대해 설명해주세요.

**[원본 답변]**두 어노테이션 모두 IoC 컨테이너에 Bean을 등록하기 위해 사용합니다

@Component : 개발자가 작성한 class를 기반으로 실행시점에 인스턴스 객체를 1회(싱글톤) 생성합니다

@Controller, @Service, @Repository 는 모두 @Component 이며 실행시점에 자동으로 의존성을 주입합니다

@Bean : 개발자가 작성한 method를 기반으로 메서드에서 반환하는 객체를 인스턴스 객체로 1회(싱글톤) 생성합니다

### Q. POJO란 무엇인가요? Spring Framework에서 POJO는 무엇이 될 수 있을까요?

**[원본 답변]** POJO는 프레임워크 인터페이스, 클래스를 구현하거나 확장하지 않은 단순한 클래스로 Java에서 제공하는 API 외에 종속되지 않습니다. 특정 환경에 종속되지 않아 코드가 간결하고 테스트 자동화에 유리합니다. 스프링에서는 도메인과 비즈니스 로직을 수행하는 대상이 POJO대상이 될 수 있습니다.

### Q. Spring Web MVC에서 요청 마다 Thread가 생성되어 Controller를 통해 요청을 수행할텐데, 어떻게 1개의 Controller만 생성될 수 있을까요?

**[원본 답변]**@Controller 어노테이션을 타고 들어가보면 @Component라는 어노테이션이 붙어 있습니다. 따라서 컨트롤러는 IoC컨테이너에 등록되어 Spring bean으로 관리됩니다. Spring의 빈 생성 전략의 기본은 싱글턴입니다. IoC컨테이너에 Controller Bean은 싱글턴 전략에 의해 1개만 존재하고 Application이 Init 되는 시점에 초기화됩니다. 그리고 실제 사용되는 시점에 의존성 주입을 통해서 사용됩니다. 요청시마다 새로 Bean을 생성해서 사용하는 것이 아닌 이미 생성되어있는 Bean을 가져다 쓰게됨으로써 여러개의 Thread에서 Contoller를 사용해도 1개의 동일한 컨트롤러인 것입니다. 만약 요청이 올때마다 새로운 컨트롤러가 생기길 바란다면 Bean scope를 Singleton 이 아닌, request 등으로 설정하면 요청이 들어올 때 혹은 지정한 전략마다 새로운 컨트롤러 Bean이 생기도록 할 수 있습니다.

### Q. Spring WEB MVC의 근간에는 Java Servlet 이 있는데요. Spring 은 Servlet을 어떻게 구성해서 이를 구현했을까요?

**[원본 답변]** Servlet은 Java로 웹페이지를 구성할 때 동적으로 웹페이지를 구성해주는 자바 클래스 입니다. Spring에서도 이 Servlet을 사용하고 있지만 특성이 조금 다릅니다. 기본적으로 Java의 Servlet은 하나의 Request에 대해서 하나의 Servlet을 생성합니다. 이 방법은 간단하고 직관적이지만 Servlet이 많이 생성되면 관리하기 힘들어지는 단점이 있습니다. 반면 Spring의 경우에는 DispatcherServlet이라는 FrontController 패턴을 사용해서 중앙에서 하나의 Servlet이 요청을 받아서 HandlerMapping을 통해 그에 맞는 컨트롤러로 분배하는 방식을 사용합니다. 이렇게 할 경우 하나의 객체에서 모든 요청을 먼저 처리하기 때문에 재사용성 및 유연한 매핑, 인터셉터의 사용, 관리의 용이성 등이 있겠습니다.

### Q. Filter는 Servlet의 스펙이고, Interceptor는 Spring MVC의 스펙입니다. Spring Application에서 Filter와 Interceptor를 통해 예외를 처리할 경우 어떻게 해야 할까요?

**[원본 답변]**Filter는 DispatcherServlet 외부에 존재하기 때문에 예외가 발생했을 때 ErrorController에서 처리해야 합니다. 하지만 Interceptor는 DispatcherServlet 내부에 존재하기 때문에 @ControllerAdvice를 적용해서 처리할 수 있습니다.

### Q. Spring Application을 구동할 때 메서드를 실행시키는 방법에 대해 설명해주세요.

**[원본 답변]**CommandLineRunner, ApplicationRunner를 구현한 클래스를 만들어서 실행시키는 2가지 방법이 있습니다. 또한 Spring의 ApplicationEvent를 사용한 방법, @Postconstruct를 사용한 방법, InitializingBean 인터페이스를 구현하는 방법, @Bean의 initMethod를 사용한 방법이 있습니다.

### Q. 의존성과 설정값을 생성자 인자로 주입해야 하는 이유에 대해 설명해주세요.

**[원본 답변]**모든 의존성을 생성자를 통해 주입하면, 인스턴스 생성 시 즉시 어떠한 동작을 실행할 수 있습니다. 또한 추가적인 설정은 필요하지 않으며, 뜻하지 않게 의존성과 설정값을 빠뜨리는 일이 발생하지 않고 테스트에도 용이합니다.