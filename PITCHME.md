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
- No Surprises |

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


// Define main class in Gradle build file
springBoot {
    mainClass = 'kotlin.demo.SimpleApplicationKt'
}
```
@[1-6](Define Spring Boot Application)
@[3-6](Declare main class in Gradle build file)

+++
## Spring - JPA Data Repositories
``` Kotlin
data class Movie(@Id val id: Long, val title: String, val director: String)

interface MovieRepository : CrudRepository<Movie, Long> {
	fun findByTitle(title: String): List<Movie>
  fun findByDirector(director: String): List<Movie>
}
```
@[1](Define entity as data class)
@[3-6](Define JPA Repository)

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
- Kotlin is now supported by Spring Initializr
- Simplifies the creation of Spring Boot projects
- Incorporated in IntelliJ but available online via https://start.spring.io/

+++
## Spring Initializr Demo
- Live Demo
  - Creation of simple application
  - Data classes
  - MongoDB Access
  - MVC Controller

+++
## Spring - Reactive Programming
- Spring Web Reactive
  - Alternative to Spring Web MVC
- Spring Framework 5
- Reactive Streams

+++


---
## Alternative Frameworks
- Ktor
- Spark

+++
## Ktor Example




+++
## Spark Example



+++
## Summary


++
## Useful Links
