Below are structured real-time use-case explanations with code examples, summaries, and interviews Q\&A for each topic you listed.

---

## 1. What is Spring?

**Use‑case & Explanation**

* ✅ Enterprise-grade Java framework enabling easier integration, configuration, and extensibility in apps.
* 🚀 Provides modules for IoC/DI, JDBC, MVC, AOP, Security, and more.
* 🧩 Helps build loosely-coupled, testable, maintainable applications.
* 🤖 Powers microservices with Spring Boot support.
* 🔧 Reduces boilerplate via annotations and auto‑configuration.

**Summary**
Spring is a comprehensive Java framework that standardizes enterprise development by offering core services like inversion of control, AOP, security, and MVC. It promotes modularity, configuration flexibility (XML, Java, annotations), and integrates well with databases, messaging, and web services. Spring Boot simplifies setup with embedded servers and convention-over-configuration. It fosters testable, maintainable, production-grade systems suitable for cloud deployment.

```java
@Configuration
@ComponentScan("com.example.app")
public class AppConfig { }

@Component
public class HelloService {
    public String sayHello() { return "Hello, Spring!"; }
}

public class Demo {
    public static void main(String[] args) {
        var ctx = new AnnotationConfigApplicationContext(AppConfig.class);
        var svc = ctx.getBean(HelloService.class);
        System.out.println(svc.sayHello()); // prints Hello, Spring!
    }
}
```

**Interview Q\&A**

1. **Q**: Why choose Spring over older frameworks like Struts or plain Servlets?
   **A**: Spring offers holistic support for DI, AOP, transaction management, REST, security, and testability, reducing boilerplate and enhancing modularity.

2. **Q**: What are major Spring modules?
   **A**: Spring Core (IoC/DI), AOP, Spring MVC, Spring Data/JDBC/JPA, Spring Security, Spring Boot, Spring Cloud, etc.

3. **Q**: Can you use Spring without Spring Boot?
   **A**: Absolutely—Spring Core works standalone. Boot is optional and just streamlines configuration with embedded web server and starters.

---

## 2. Jakarta EE vs Spring

**Use‑case & Explanation**

* 🔄 Both support dependency injection and web capabilities.
* 🤝 Jakarta EE uses standards (Servlet, CDI, JPA) via containers like Payara/WildFly.
* ⚙️ Spring uses its own contracts and often easier microservice-first stack via Boot.
* 📦 Deployment: Spring Boot produces self‑contained JARs; Jakarta EE runs on external app‑servers.
* 🧪 Testing: Spring Boot has embedded-test support via @SpringBootTest.

**Summary**
Jakarta EE and Spring are industry-leading Java ecosystems. Jakarta EE emphasizes standardized APIs with vendor-neutral containers (EJB, CDI, JAX-RS). Spring offers richer ecosystem, convention-over-configuration approach via Boot, fast startup, and standalone deployments. Use EE for pure-Java-standards consistency across vendors; Spring for flexibility, cloud readiness, and extensive ecosystem. Both can interop—Spring supports JPA, JAX-RS, etc.

```java
// Jakarta EE example: JAX-RS endpoint
@Path("/hello")
@RequestScoped
public class HelloResource {
    @GET @Produces(MediaType.TEXT_PLAIN)
    public String hello() { return "Hello, Jakarta EE!"; }
}
```

**Interview Q\&A**

1. **Q**: When to pick Jakarta EE over Spring?
   **A**: Choose Jakarta EE when you need vendor-neutral portability and strict Java EE standard compliance.

2. **Q**: Can Spring Boot run on an external servlet container?
   **A**: Yes, by packaging as WAR and configuring a Servlet initializer instead of embedding Tomcat/Jetty.

3. **Q**: Does Spring fully support JPA?
   **A**: Yes—Spring Data JPA supports JPA implementations like Hibernate, EclipseLink.

---

## 3. Introduction to Spring Core

**Use‑case & Explanation**

* 💡 Foundation of Spring, providing IoC container, bean lifecycle, DI.
* 📚 Includes ApplicationContext (BeanFactory), resource loading, property support.
* 🔧 Uses @Component, XML, or Java @Bean for bean definitions.
* 👁️‍🗨️ Supports environment abstraction, profiles, and event publishing.
* 🔄 Manages configuration and wiring across layers.

**Summary**
Spring Core is the heart of the framework—its IoC container instantiates, configures, and connects beans via various metadata sources. It offers ApplicationContext interface with lifecycle callbacks, property resolution, resource access, and event system. Beans can be discovered by scanning or declared manually. This abstraction simplifies program architecture by delegating wiring and lifecycle management.

```java
@Configuration
public class CoreConfig {
  @Bean
  public GreetingService greetingService() { return new GreetingServiceImpl(); }
}
public class App {
  public static void main(String[] args) {
    var ctx = new AnnotationConfigApplicationContext(CoreConfig.class);
    System.out.println(ctx.getBean(GreetingService.class).greet());
  }
}
```

**Interview Q\&A**

1. **Q**: What’s the difference between BeanFactory and ApplicationContext?
   **A**: ApplicationContext extends BeanFactory—it eagerly loads singleton beans, supports messages, events, and resource access, not just lazy instantiation.

2. **Q**: How do you define beans?
   **A**: Via XML, Java @Configuration methods with @Bean, or component-scanning using stereotypes (@Component, @Service).

3. **Q**: Can beans be prototype-scoped?
   **A**: Yes. Use `@Scope("prototype")` for multi-instance behavior instead of singleton.

---

## 4. Introduction to IoC & DI

**Use‑case & Explanation**

* 🧩 IoC reverses control of object creation to framework.
* 🔌 DI injects dependencies (constructor, setter, field).
* 💻 Simplifies testing—swap implementations easily.
* 🔄 Promotes separation of concerns and loose coupling.
* 💡 Increases modularity and configuration flexibility.

**Summary**
Inversion of Control (IoC) delegates responsibility for managing object lifecycle and dependencies to the Spring container. Dependency Injection (DI) is an implementation of IoC—framework injects collaborators rather than objects manually wiring them. DI improves modularity, testability, and easier configuration. It supports constructor, setter, and field injection.

```java
@Component class A { }
@Component class B {
  private final A a;
  @Autowired
  public B(A a) { this.a = a; }
}
```

**Interview Q\&A**

1. **Q**: Difference between setter and constructor injection?
   **A**: Constructor injection is recommended: makes dependencies required and immutable. Setter injection is optional or circular dependencies.

2. **Q**: Can DI be done without a framework?
   **A**: Yes, manually via new and passing in dependencies. IoC frameworks automate it and scale better.

3. **Q**: When would you use field injection?
   **A**: Rarely—only for test code or highly non‑modular cases. Constructor injection remains preferred.

---

## 5. Demo of IoC & DI

**Use‑case & Explanation**

* 🏗️ Show a real component wiring scenario.
* 👨‍💻 Demonstrates automatic instantiation and injection.
* 🧪 Includes a mock configuration for testing.
* ✅ Shows one-to-many implementation injection.
* 🔁 Wiring via Java config or XML.

**Summary**
In this demo, we define interfaces and implementations wired by Spring. We show constructor injection and switching implementations via profiles or config. This demonstrates loose coupling and easy test swapping. The demo proves DI in action, where class B doesn’t create A but receives it through IoC container wiring.

```java
public interface MessageProvider { String getMessage(); }
@Component @Profile("dev")
public class DevMessageProvider implements MessageProvider { public String getMessage() { return "Dev"; } }
@Component @Profile("prod")
public class ProdMessageProvider implements MessageProvider { public String getMessage() { return "Prod"; } }

@Component class UseMessage {
  private final MessageProvider provider;
  @Autowired
  public UseMessage(MessageProvider provider) { this.provider = provider; }
  public void show() { System.out.println(provider.getMessage()); }
}

public class Demo {
  public static void main(String[] args) {
    System.setProperty("spring.profiles.active", "dev");
    var ctx = new AnnotationConfigApplicationContext("com.example");
    ctx.getBean(UseMessage.class).show(); // prints "Dev"
  }
}
```

**Interview Q\&A**

1. **Q**: What if multiple beans match DI type?
   **A**: Use @Qualifier or @Primary to resolve ambiguity.

2. **Q**: How to change profile at runtime?
   **A**: Set spring.profiles.active via System property, environment var, or application.properties.

3. **Q**: How to inject a collection of beans?
   **A**: `@Autowired List<MyInterface> allImpls;` injects all matching beans.

---

## 6. Advantages of IoC & DI

**Use‑case & Explanation**

* 🔁 Decoupling implementation from usage.
* 🧪 Easier unit testing via mocks.
* 🔄 Easier to swap implementations in deployment.
* 🔍 Centralized wiring and lifecycle management.
* 🌱 Encourages single responsibility and cleaner modules.

**Summary**
IoC and DI improve code quality by decoupling components and externalizing wiring. This boosts modularity, simplifies testing, and allows flexible configuration without code changes. This style aids maintainability, supports evolving requirements, and enables easier refactoring. In large apps, IoC avoids tight coupling and deep dependency chains.

```java
@Service class AService { }
@Service class BService {
  private final AService aService;
  @Autowired BService(AService a) { this.aService = a; }
}
```

**Interview Q\&A**

1. **Q**: How does DI enhance testing?
   **A**: You can inject mocks or stubs instead of real implementations in unit tests.

2. **Q**: What inversion of control containers are besides Spring?
   **A**: Google Guice, Jakarta CDI (Weld/OpenWebBeans), PicoContainer.

3. **Q**: Any disadvantages to DI?
   **A**: Some learning curve, container complexity, potential runtime misconfiguration.

---

## 7. Introductions to Beans, Context, and SpEL

**Use‑case & Explanation**

* 🧩 "Bean": managed object in Spring container.
* 🌳 "Context": ApplicationContext gives lifecycle, DI, event, resource APIs.
* 🔤 SpEL: Spring Expression Language used in annotations like `@Value`.
* 🔍 SpEL supports method calls, arithmetic, collection access.
* 🛠️ Leverages SpEL for dynamic configuration and condition evaluation.

**Summary**
Beans are Spring-managed objects declared via XML, Java, or annotations. The ApplicationContext is the central factory for beans, configuration, and eventing. SpEL empowers dynamic expressions such as property access, math, conditional, and calling custom functions. Used in `@Value`, bean definitions, security, and conditional loading.

```java
@Value("#{T(java.lang.Math).random() * 100}")
private double rand;

@Value("#{systemProperties['user.home']}")
private String homeDir;
```

**Interview Q\&A**

1. **Q**: What’s SpEL vs JSP EL or OGNL?
   **A**: SpEL is for Spring metadata config with method execution, arithmetic, indexing—embedded in annotations/XML.

2. **Q**: Can SpEL reference other beans?
   **A**: Yes: `@Value("#{otherBean.someProperty}")`

3. **Q**: What happens on SpEL parse error?
   **A**: Context initialization fails with ExpressionException.

---

## 8. Introduction to Spring IoC Container

**Use‑case & Explanation**

* 🏛️ IoC Container manages bean instantiation, wiring, lifecycle.
* 📦 Bean definitions loaded via ApplicationContext from Java, XML, or classpath.
* 🔁 Supports scopes: singleton, prototype, request, session.
* 🔄 Supports lifecycle callbacks like `@PostConstruct` & `@PreDestroy`.
* 🔍 Also supports Aware interfaces, events, and custom post‑processors.

**Summary**
The Spring IoC Container, represented by BeanFactory/ApplicationContext, is the runtime environment where beans live. It uses metadata to instantiate, configure, and link beans, supports scopes and lifecycle, and adds cross‑cutting services like events. Developers define beans via Java classes, XML, or scan for annotated components. The container handles dependency resolution, lifecycle hooks, and context-wide features.

```java
@Configuration
public class Cfg {
  @Bean(initMethod="init", destroyMethod="cleanup")
  public ResourceHolder rh() { return new ResourceHolder(); }
}
public class Demo {
  var ctx = new AnnotationConfigApplicationContext(Cfg.class);
  ctx.close(); // triggers destroyMethod
}
```

**Interview Q\&A**

1. **Q**: How do prototype scoping and container shutdown differ?
   **A**: Prototype beans aren’t tracked by context, so destroy callbacks aren’t called automatically.

2. **Q**: What is BeanPostProcessor?
   **A**: Interface to apply custom logic before/after bean initialization.

3. **Q**: What is BeanFactoryPostProcessor?
   **A**: Modifies bean definitions before container bean instantiation begins.

---

## 9. “Introduction to Spring Framework” Quiz

1. **Q**: What annotation is used to mark a configuration class?

   * **A**: `@Configuration`

2. **Q**: How do you declare a bean with XML config?

   * **A**: `<bean id="userService" class="com.example.UserService"/>`

3. **Q**: What does `@Autowired` do?

   * **A**: Marks a field, constructor, or setter for dependency injection by type.

---

## 10. Creating Beans inside Spring Context

**Use‑case & Explanation**

* 📦 Beans defined through Java @Bean or @Component scanning.
* 🌲 Configuration classes act as bean factories.
* ♻️ You can create beans programmatically using `registrar.registerBean()`.
* 🔁 Beans can be conditionally created via `@Conditional`, `@Profile`.
* 🧪 Useful for dynamic or modular bean registration.

**Summary**
Spring allows bean registration via annotations and XML. Java-based configuration via `@Bean` is concise and type-safe. `@Component` with scanning auto-detects. You can also manually register beans at runtime using `GenericApplicationContext.registerBean()`, enabling dynamic modules or plugin patterns. Conditional annotations help fine-tune when beans exist.

```java
public class DynamicDemo {
  public static void main(String[] args) {
    var ctx = new AnnotationConfigApplicationContext();
    ctx.register(DynamicDemo.class);
    ctx.registerBean("foo", String.class, () -> "Dynamically registered");
    ctx.refresh();
    System.out.println(ctx.getBean("foo"));
    ctx.close();
  }
}
```

**Interview Q\&A**

1. **Q**: How to register beans at runtime?
   **A**: Use `registerBean()` on `GenericApplicationContext` before `refresh()`.

2. **Q**: What’s the advantage of Java @Bean over XML?
   **A**: Type safety, easier refactoring, IDE support, and conditional registration via code.

3. **Q**: Can two beans share the same class?
   **A**: Yes—give them different names via `@Bean("name")` or `@Component("...")`.

---

Below are detailed explanations with real-world coding examples, bullet-point highlights, concise 5‑line summaries, sample code, followed by 3 interview-style Q\&As for each topic.

---

## 🔧 Installation of Maven

**Use‑case & Highlights**

* Enables dependency management and build lifecycle automation.
* Integrates with CI/CD tools like Jenkins or GitHub Actions.
* Standardizes project structure across the organization.
* Supports transitive dependencies and plugins (Surefire, Shade).
* Facilitates multi-module project builds.

**Summary**
Maven streamlines Java project builds using a declarative `pom.xml`. It handles downloading dependencies, compilation, testing, packaging, and deployment phases automatically. With its conventions, teams minimize configuration and maximize consistency. It also supports plugin-rich extensions for reporting, docs, and deployments. Maven is a staple in enterprise and open‑source Java projects.

```bash
# On Debian/Ubuntu:
sudo apt update
sudo apt install maven

# Verify:
mvn -v
```

**Interview Q\&A**

1. **Q**: What are Maven lifecycle phases?
   **A**: Default phases include `validate`, `compile`, `test`, `package`, `verify`, `install`, `deploy`.

2. **Q**: What's the difference between `install` and `deploy`?
   **A**: `install` puts the artifact in your local repo; `deploy` pushes it to a remote, shared repo.

3. **Q**: How do you skip tests during build?
   **A**: Use `-DskipTests` or `-Dmaven.test.skip=true`.

---

## 🧠 IntelliJ IDEA Ultimate

**Use‑case & Highlights**

* Full stack support for Spring Boot, JPA, REST APIs.
* Built-in UI for running/debugging maven/gradle projects.
* Smart refactoring, code inspection, and database tools.
* Includes profiling, API testing, and Docker integration.
* Supports advanced frameworks (Micronaut, Quarkus, Jakarta EE).

**Summary**
IntelliJ IDEA Ultimate is a premium IDE tailored for enterprise Java development. It offers deep integration with frameworks, container tools, and databases. Intelligent code analysis and fast refactoring support large codebases efficiently. GUI tools simplify version control, SQL editing, and live debugging. It's a favorite among developers building sophisticated applications.

```bash
# Download installer from JetBrains website, run:
chmod +x ideaIU-2025.1.sh
./ideaIU-2025.1.sh

# Open and select "Import Maven Project" to get started.
```

**Interview Q\&A**

1. **Q**: What is the difference between Community and Ultimate?
   **A**: Ultimate supports enterprise frameworks (Spring, JPA, REST, DB tools); Community focuses on core Java and Kotlin.

2. **Q**: Can IntelliJ import Gradle projects too?
   **A**: Yes—direct support for both Maven and Gradle with synchronized project automation.

3. **Q**: How to use live templates?
   **A**: Via `Preferences → Editor → Live Templates`; type abbreviations like `psvm` to generate code.

---

## 📂 Creating Maven Project

**Use‑case & Highlights**

* Bootstraps standardized directory layout using `archetype`.
* Auto-generates `pom.xml` with default settings.
* Integrates seamlessly with IDEs like IntelliJ/Eclipse.
* Sets groupId, artifactId, version upfront.
* Enables multi-module structure with ease.

**Summary**
Creating a Maven project via `archetype:generate` provides a templated skeleton with `src/main/java`, `src/test/java`, and `pom.xml`. It preconfigures dependencies, packaging, and plugins. IDEs recognize the structure immediately. Ensures team-wide project consistency. Lays foundation for fast development and CI integration.

```bash
mvn archetype:generate \
  -DgroupId=com.example \
  -DartifactId=demo-app \
  -DarchetypeArtifactId=maven-archetype-quickstart \
  -DinteractiveMode=false
cd demo-app
mvn clean package
```

**Interview Q\&A**

1. **Q**: How to change default JDK version?
   **A**: Update `<maven.compiler.source>` and `<maven.compiler.target>` in `pom.xml`.

2. **Q**: What’s an archetype?
   **A**: A project template structure for quick start.

3. **Q**: How to convert it to multi-module later?
   **A**: Create parent POM packaging, add `<modules>` entries, and move child projects into subfolders.

---

## 🛠️ Creating Beans using @Bean Annotation

**Use‑case & Highlights**

* Defines beans via Java methods instead of XML.
* Supports returning 3rd-party or custom class instances.
* Enables method-level customization (init, destroy).
* Supports conditional bean creation with `@Conditional`.
* Facilitates reusability and testability.

**Summary**
`@Bean` annotates methods inside `@Configuration` classes to define beans explicitly. It’s ideal for 3rd-party classes or when a specific initialization is needed. Beans support lifecycle callbacks and can accept dependencies via method parameters. It's type-safe, refactor-friendly and aligns with Java configuration philosophy over XML.

```java
@Configuration
public class AppConfig {
  @Bean
  public DateTimeFormatter dtf() {
    return DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
  }
}

public class Demo {
  public static void main(String[] args) {
    var ctx = new AnnotationConfigApplicationContext(AppConfig.class);
    var dtf = ctx.getBean(DateTimeFormatter.class);
    System.out.println(dtf.format(LocalDateTime.now()));
  }
}
```

**Interview Q\&A**

1. **Q**: What’s the difference between `@Bean` and `@Component`?
   **A**: `@Bean` is used in Java config; `@Component` is for classpath scanning with default constructor.

2. **Q**: How to define destroy method?
   **A**: Use `@Bean(initMethod="init", destroyMethod="cleanup")`.

3. **Q**: Can `@Bean` method take parameters?
   **A**: Yes—Spring injects other beans by matching parameter types.

---

## 🤖 Say Hello to your new AI Coding Companion

**Use‑case & Highlights**

* Bootstraps a CLI or REST command to chat with an AI agent.
* Integrates with GPT via HTTP client as a Spring bean.
* Enhances developer workflow with code suggestions.
* Uses DI to inject AI service in controllers or services.
* Supports dynamic prompt configuration via SpEL or `@Value`.

**Summary**
This topic inspires building a simple AI-powered messaging service in Spring. It shows injecting a GPT client bean and exposing a `/chat` REST API endpoint. It demonstrates DI, HTTP calls, and incorporating dynamic service configuration. A practical example of leveraging Spring to wrap an external AI. Great intro to microservice integration.

```java
@Configuration
class AiConfig {
  @Bean public OpenAiClient aiClient() {
    return new OpenAiClient("YOUR_API_KEY");
  }
}

@RestController
class ChatController {
  private final OpenAiClient ai;
  @Autowired ChatController(OpenAiClient ai) { this.ai = ai; }

  @PostMapping("/chat")
  public String chat(@RequestBody String msg) {
    return ai.send(msg);
  }
}
```

**Interview Q\&A**

1. **Q**: How to secure the API key?
   **A**: Use Spring Boot’s `@Value("${openai.key}")` pulling from environment variables or Vault.

2. **Q**: How to test ChatController?
   **A**: Use `@WebMvcTest` with MockBean for `OpenAiClient`, mock its `send()`.

3. **Q**: How to add retry logic on failures?
   **A**: Integrate `spring-retry` or Resilience4j with `@Retryable`.

---

## 🧩 Understanding NoUniqueBeanDefinitionException in Spring

**Use‑case & Highlights**

* Thrown when multiple bean candidates match a required type.
* Occurs with `@Autowired` by type without qualifiers.
* Indicates ambiguous wiring in DI context.
* Fixable by `@Qualifier` or `@Primary`.
* Helps prevent runtime mis-wiring errors.

**Summary**
`NoUniqueBeanDefinitionException` is thrown when DI attempts to inject a type but finds more than one matching bean. It alerts developers to ambiguity in bean configuration. The solution is to mark one bean as `@Primary` or use `@Qualifier` to specify which bean to inject. It's critical for avoiding unexpected runtime behavior in complex contexts with multiple implementations.

```java
@Component class A1 implements Service {}
@Component class A2 implements Service {}

@Component class UseSvc {
  @Autowired @Qualifier("a2") Service svc;
}
```

**Interview Q\&A**

1. **Q**: How to fix this exception automatically?
   **A**: Use `@Primary` on the default bean implementation.

2. **Q**: Can you specify qualifier at injection point?
   **A**: Yes: `@Qualifier("beanName")`.

3. **Q**: What if using constructor injection?
   **A**: Qualifier goes on constructor parameter: `public C(@Qualifier("x") Service s)`.

---

## 📝 Providing a custom name to the bean

**Use‑case & Highlights**

* Names beans explicitly via `@Component("myBean")` or `@Bean("myBean")`.
* Helps when multiple beans of same type exist.
* Makes XML references and `@Qualifier` simpler.
* Aids readability and maintainability.
* Supports legacy integration with specific bean IDs.

**Summary**
Custom bean naming gives explicit control over Spring-managed instance identification. By default, bean name is derived from class or method. Specifying names is essential when multiple implementations of an interface exist. Named beans support clearer wiring and reduce ambiguity. This practice aids debugging in larger applications.

```java
@Component("fastSvc")
public class FastService implements TaskService {}

@Component("slowSvc")
public class SlowService implements TaskService {}
```

**Interview Q\&A**

1. **Q**: What is default bean name for `@Component`?
   **A**: The uncapitalized class name, e.g., `fastService`.

2. **Q**: Can you assign multiple names?
   **A**: Yes: `@Component({"bean1","aliasBean"})`.

3. **Q**: How to inject named bean?
   **A**: Use `@Qualifier("beanName")` along with `@Autowired`.

---

## ⭐ Understanding @Primary Annotation in Spring

**Use‑case & Highlights**

* Marks a bean as default when duplicates exist.
* Eliminates need for `@Qualifier` in many cases.
* Useful in library-provided fallback beans.
* Works with `@Configuration` and `@Component`.
* Can be overridden with explicit qualifiers.

**Summary**
`@Primary` marks a bean as preferred when multiple beans of the same type are candidates. This solves `NoUniqueBeanDefinitionException` without qualifiers. Though convenient, qualifiers should still be used when explicit control is desired. `@Primary` provides safe fallback for most injection scenarios and helps reduce boilerplate.

```java
@Component
@Primary
public class DefaultSvc implements TaskService {}

@Component
public class SpecialSvc implements TaskService {}
```

**Interview Q\&A**

1. **Q**: Which wins: `@Primary` or `@Qualifier`?
   **A**: `@Qualifier` always overrides `@Primary`.

2. **Q**: Can you combine them?
   **A**: Yes—primary on one, qualifier to narrow down in edge cases.

3. **Q**: Scope usage?
   **A**: Scoping unaffected—`@Primary` only concerns selection priority.

---

## 🏷️ Creating Beans using @Component Annotation

**Use‑case & Highlights**

* Auto-detected by classpath scanning.
* Frees you from manual `@Bean` definition.
* Stereotypes: `@Service`, `@Repository`, `@Controller`.
* Supports default bean naming.
* Encourages modular, decoupled design.

**Summary**
`@Component` and its stereotypes allow Spring to auto-detect and register beans during component scanning. This convention-driven approach reduces annotation clutter and promotes clear intent. Developers annotate classes once and dependencies are injected automatically. Fully supports lifecycle management, AOP, and custom naming. Widely used in modern Spring applications.

```java
@Component
public class GreetingService {
  public String greet() { return "Hi!"; }
}

@Component
public class GreetRunner implements CommandLineRunner {
  @Autowired private GreetingService svc;
  @Override public void run(String... args) { System.out.println(svc.greet()); }
}
```

**Interview Q\&A**

1. **Q**: What's the scan path?
   **A**: Defined via `@ComponentScan(basePackages="com.example")` or by default root package.

2. **Q**: Can you exclude components?
   **A**: Yes—use `excludeFilters` in `@ComponentScan`, or `@Profile`.

3. **Q**: What stereotype for DAOs?
   **A**: Use `@Repository`, which also translates DB exceptions.

---

## 📚 Stereotype Annotations in Spring

**Use‑case & Highlights**

* Classifies roles: `@Controller`, `@Service`, `@Repository`, `@Component`.
* Adds role-specific behavior (e.g. exception translation).
* Improves readability/maintenance.
* Enables automatic scanning and AOP application.
* Semantic clarity in architecture.

**Summary**
Spring’s stereotype annotations provide semantic meaning and enable classpath scanning and AOP features. `@Controller` marks web controllers, `@Service` for business logic, `@Repository` for DAO with exception translation, and `@Component` for generic. These annotations help clearly define responsibilities and allow the container to apply appropriate processing.

```java
@Repository
public class UserRepo {
  public User find(String id) { /* JDBC or JPA logic */ }
}

@Service
public class UserService {
  @Autowired private UserRepo repo;
  public User getUser(String id) { return repo.find(id); }
}

@Controller
public class UserController {
  @Autowired private UserService svc;
  @GetMapping("/user")
  public User get(@RequestParam String id) { return svc.getUser(id); }
}
```

**Interview Q\&A**

1. **Q**: How is `@Repository` special?
   **A**: It triggers Spring’s exception translation for persistence errors.

2. **Q**: What AOP proxies controllers?
   **A**: Spring MVC uses `@Controller` to map HTTP handlers; proxies not applied by default.

3. **Q**: Can service layer have transaction management?
   **A**: Yes—use `@Service` + `@Transactional` to demarcate business transactions.

---

Let me know if you'd like me to add or refine any topic! 😊
