## Kotlin REST Applications
#### Andy Bowes

+++
## Benefits of Kotlin
- Existing Java knowledge is transferable |
  - Build & deploy pipeline is unchanged
  - Gradle & Maven Integration
- Expressiveness |
  - Removes Java boilerplate
  - Clear & Concise Code

---
## Spring Integration
- Kotlin & Spring just works |
  - All the familiar annotations & patterns
- Added dedicated support in Spring Framework 5.0 |
  - Pivotal are committed to Kotlin
  - Sebastien Deleuze champions the language
- Spring has tagged many methods as @NotNull |
- Added Kotlin extension functions |

+++
## Spring

- No need to declare your bean class as open anymore |
- Classes with these annotations are automatically 'open' |
  - @Component, @Async, @Transactional, @Cacheable
  - @Controller, @RestController, @Service or @Repository are automatically opened since these annotations are meta-annotated with @Component.

---
## Spring Boot

``` Kotlin
@SpringBootApplication
open class SimpleApplication

fun main(args: Array<String>) {
    SpringApplication.run(Application::class.java, *args)
}
```
@[1-2](Define Spring Boot Application)
@[4-6](Define main class as function)

---
## Spring - JPA Data Repositories
``` Kotlin
@Entity
data class Movie(@Id val id: Long,
          val title: String,
          val director: String)

interface MovieRepository : CrudRepository<Movie, Long> {
	fun findByTitle(title: String): List<Movie>
  fun findByDirector(director: String): List<Movie>
}
```
@[1-4](Define entity as data class)
@[6-9](Define JPA Repository)

---
## Spring Web MVC

``` Kotlin
@RestController
class MovieController (val repository:MovieRepository) {

	@GetMapping("/")
  fun findAll() = repository.findAll()

	@GetMapping("/{title}")
	fun findByTitle(@PathVariable title:String)
		= repository.findByTitle(title)

  @GetMapping("/director/{director}")
  	fun findByDirector(@PathVariable director:String)
  		= repository.findByDirector(director)
}
```

+++
## Spring Initializr
- Simplifies the creation of Spring Boot projects
- Available online via https://start.spring.io/
- Kotlin is now supported by Spring Initializr
- Incorporated in IntelliJ

---
## Spring Initializr Demo
- Live Demo
  - Creation of simple application
  - Data classes
  - MongoDB Access
  - MVC Controller

+++
## Deployment
- Kotlin compiles to Java byte-code |
- Builds typical Java JAR file |
- Add Kotlin Dependencies |
  - kotlin-stdlib
  - additional libs for Java 7 & Java 8 features

---
## Docker Deployment
- Identical to Java Spring application
- Example Dockerfile
``` Shell
FROM openjdk:alpine
EXPOSE 8080
ADD build/libs/myapplication-0.0.1.jar app.jar
RUN sh -c 'touch /kmdb.jar'
ENTRYPOINT [ "sh", "-c", "java $JAVA_OPTS -jar /app.jar" ]
```

+++
## Spring WebFlux
- Spring WebFlux |
  - Reactive Alternative to Spring Web MVC
- Spring Framework 5 |
  - Reactor
- Reactive Streams |
  - Mono - Single elements
  - Flux - Streams of elements
- MongoDB or Redis Database Support |

---
## Reactive Controller Mapping
- Use Routing DSL to provide mapping of URIs to functions
``` Kotlin
{
    ("/movies" and accept(APPLICATION_JSON)).nest {
        GET("/", movieHandler::findAll)
        GET("/{id}", movieHandler::findOne)
        GET("/actor/{actor}", movieHandler::findByActor)
    }
}
```

---
## Reactive Handler Functions
``` Kotlin
@Component
class MovieHandler(val repository: MovieRepository) {
    fun findOne(req: ServerRequest) =
          ok().json().body(repository.findOne(req.pathVariable("id")))
    fun findAll(req: ServerRequest) =
         ok().json().body(repository.findAll())
    fun findByActor(req: ServerRequest) =
         ok().json().body(repository.findByActor(req.pathVariable("actor")))
}

@Repository
class MovieRepository(val template: ReactiveMongoTemplate,
                      val objectMapper: ObjectMapper) {
    fun findAll(): Flux<Movie> = template.find<Movie>(Query().with(Sort.by("title")))
    fun findOne(id: String): Mono<Movie> = template.findById<Movie>(id)
    fun findByActor(name: String): Flux<Movie> = template.findByActor<Movie>(name)
}
```
@[1-9](Define Handler functions)
@[10-16](Define Reactive Repository)

---
## Spring FU
- *Spring Fu*ntional APIs
- Experimental microframework |
  - Uses a DSL to provide functional definition of Beans & resources |
- Intended to provide a modular approach to using Spring Boot |
- Explicit configuration: minimal core, no conventions, no classpath detection |
- Run as native image with instant startup via GraalVM support |
- *Not production ready* |
---
## Spring FU Sample Application
``` Kotlin
fun main(args: Array<String>) = application {
	bean<UserRepository>()
	bean<UserHandler>()
	webflux {
		val port = if (profiles.contains("test")) 8181 else 8080
		server(netty(port)) {
			mustache()
			codecs {
				jackson()
			}
			router {
				val userHandler = ref<UserHandler>()
				GET("/", userHandler::listView)
				GET("/api/user", userHandler::listApi)
			}
		}
	}
}.run(await = true)
```
@[2-3](Define Beans)
@[5-6](Define Netty Server)
@[11-15](Define URL Mappings)
+++
## Alternative Frameworks
- Spring is not the only option |
- Integrates with other Java frameworks |
  - Spark
  - Vertx
- Kotlin specific frameworks |
  - Ktor

---
## Spark
- Lightweight, expressive framework |
  - Web or REST
  - HTTP or Web Sockets
- Kotlin support added April 2017 |

---
## Spark Example

``` Kotlin
fun main(args: Array<String>) {
  val userDao = UserDao()

  path("/users") {
    get("/:id") { req, res ->
        userDao.findById(req.params("id").toInt())
    }
    get("/email/:email") { req, res ->
        userDao.findByEmail(req.params("email"))
    }
    post("/create") { req, res ->
        userDao.save(name = req.qp("name"), email = req.qp("email"))
        res.status(201)
        "ok"
    }
  }
}
```
---
Spark/Kotlin Documentation:
> I have only worked with Kotlin for a few hours while writing this tutorial, but I’m already a very big fan of the language.
>Everything just seems to make sense, and the interoperability with Java is great.

---
## Ktor
- Lightweight framework |
- Functional interface via lambda functions |
- Asynchronous by design |
- Hosted |
  - Servlet 3.0+ (e.g. Tomcat)
- Standalone |
  - Netty or Jetty

---
## Ktor Example

- Simple JSON REST Service
``` Kotlin
fun Application.main() {
    install(DefaultHeaders)
    install(CallLogging)
    install(GsonSupport) {
        setPrettyPrinting()
    }
    ...
    routing {
        get("/v1") {
            call.respond(model)
        }
        get("/v1/item/{key}") {
            val item = repository.getItem(key)
            if (item == null)
                call.respond(HttpStatusCode.NotFound)
            else
                call.respond(item)
        }
    }
}
```

+++
## Summary
- Kotlin is a viable option for server-side development |
- Easy transition from Java |
 - Use identical build & deploy process
- Latest frameworks have inbuilt Kotlin support |
- Produces elegant & clean code |
- Try It |

+++
## Useful Links
- Spring Framework/Kotlin
  - https://spring.io/blog/2017/01/04/introducing-kotlin-support-in-spring-framework-5-0
  - https://kotlinlang.org/docs/tutorials/spring-boot-restful.html
- Spring WebFlux/Kotlin
  - https://github.com/mixitconf/mixit
- Spark http://sparkjava.com/
- Ktor http://ktor.io/
