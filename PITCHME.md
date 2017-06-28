## Introduction to Kotlin
#### Andy Bowes
### The JVM Thing
##### 20<sup>th</sup> July 2017

---
## What is ‘Kotlin’?
- ‘New’ programming language |
  - Developed since 2011, v1.0 released Feb 2016
- Developed by JetBrains |
  - Makers of IntelliJ & Android Studio
- Runs on Java Virtual Machine (JVM) |
- Open Source |

+++
## What’s wrong with plain old Java ?
- It’s verbose. |
  * Too much ‘boilerplate’ code.
- Slow to Change |
- Multiple overload methods/constructors. |
- Null Pointers |
- Class Cast exceptions |
- Functional paradigm is still a bit of an afterthought. |

+++?image=assets/images/effectiveJava_cover.jpg&size=contain
<p><span>&nbsp;</span></p>

+++
James Gosling, inventor of Java:
> I sure wish I had this book ten years ago.
> Some might think that I don't need any Java books, but I need this one.

+++
## Effective ~~Java~~ Kotlin
- Kotlin is designed as a 'better' Java |
- Directly addresses many issues from Joshua Bloch's Effective Java |
- Many of these will be highlighted during this talk |

+++
## Kotlin's Relationship to Java
- All code compiles to pure Java byte-code |
- 100% Java Interoperability |
  - Kotlin classes can invoke methods in Java classes
  - Java classes can invoke Kotlin functions
- Kotlin can use standard Java libraries |
- Allows incremental migration to Kotlin from Java |
- Deploy mixed applications as a single artifact |

---?include=slides/basicSyntax.md

---?include=slides/classes.md

---?include=slides/nulls.md

---?include=slides/smartCast.md

---?include=slides/functions.md

---?include=slides/functionalProgramming.md

---?include=slides/collections.md
