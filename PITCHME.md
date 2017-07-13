## Kotlin REST Applications
#### Andy Bowes
### The JVM Thing
##### 20<sup>th</sup> July 2017

+++
## Benefits of Kotlin
- Existing Java knowledge is transferable |
  - Build & deploy pipeline is unchanged
  - Gradle & Maven Integration
- Expressiveness |
  - Removes Java boilerplate
  - Clear & Concise Code
- Coroutines |
  - Experimental in Kotlin v1.1
  - Simplification of asynchronous coding
  - Lightweight threading

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

+++
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

+++
## Spring - JPA Data Repositories
``` Kotlin
@Entity
data class Movie(@Id val id: Long, val title: String, val director: String)

interface MovieRepository : CrudRepository<Movie, Long> {
	fun findByTitle(title: String): List<Movie>
  fun findByDirector(director: String): List<Movie>
}
```
@[1-2](Define entity as data class)
@[4-7](Define JPA Repository)

+++
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

+++
## Spring Initializr Demo
- Live Demo
  - Creation of simple application
  - Data classes
  - MongoDB Access
  - MVC Controller

+++
## Deployment
- Kotlin compiles to Java byte-code
- Needs Kotlin Standard Library
-

+++
## Docker Deployment
- Identical to Java Spring application
- Dockerfile
``` Shell
FROM openjdk:alpine
EXPOSE 8080
ADD build/libs/kmdb-0.0.1.jar kmdb.jar
RUN sh -c 'touch /kmdb.jar'
ENTRYPOINT [ "sh", "-c", "java $JAVA_OPTS -jar /kmdb.jar" ]
```

+++
## Spring - Reactive Programming
- Spring Web Reactive |
  - Alternative to Spring Web MVC
- Spring Framework 5 |
- Reactive Streams |
  - Mono - Single elements
  - Flux - Streams of elements
+++
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

## Reactive Handler Functions
``` Kotlin
@Component
class MovieHandler(val repository: MovieRepository) {
    fun findOne(req: ServerRequest) = ok().json().body(repository.findOne(req.pathVariable("id")))
    fun findAll(req: ServerRequest) = ok().json().body(repository.findAll())
    fun findByActor(req: ServerRequest) = ok().json().body(repository.findByActor(req.pathVariable("actor")))
}

@Repository
class MovieRepository(val template: ReactiveMongoTemplate,
                      val objectMapper: ObjectMapper) {
    fun findAll(): Flux<Movie> = template.find<Movie>(Query().with(Sort.by("title")))
    fun findOne(id: String): Mono<Movie> = template.findById<Movie>(id)
    fun findByActor(id: String): Flax<Movie> = template.findByActor<Movie>(id)
}
```
@[1-6](Define Handler functions)
@[8-14](Define Reactive Repository)
---
## Alternative Frameworks
- Spring is just framework |
- Integrates with other Java frameworks |
  - Spark
  - Vertx
- Kotlin specific frameworks |
  - Ktor

+++
## Spark Example

``` Kotlin
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
```

+++
## Ktor Example

``` Kotlin

```

+++
## Summary
- Kotlin is a viable option for server-side development |
-
-

++
## Useful Links
