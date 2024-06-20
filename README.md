## Entities:

```java
//Put this before the class
import jakarta.persistence.*;
import jakarta.persistence.Entity;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

@Entity
@Getter
@Setter
@NoArgsConstructor
//This is for the id
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;

//For the child class
@OneToMany (mappedBy = "parent")
private List<Child> children;

//For the parent class
@ManyToOne

//This relation
----> 
//Don't put anything on the right side of the arrow also don't put mappedBy on the left side

//Non nullable
@NonNull

//Unique
@Unique

//Column name
@Column(name = "column_name")


//cascade one 1 -> * put cascade on the 1 side
//Cascade presist remove all and explained
(cascade = CascadeType.PERSIST) //When the parent is saved, the child is saved

(cascade = CascadeType.REMOVE) //When the parent is removed, the child is removed

(cascade = CascadeType.ALL) //When the parent is saved, the child is saved and when the parent is removed, the child is removed

//Fetch type
(fetch = FetchType.LAZY) //Lazy loading, only load when needed

(fetch = FetchType.EAGER) //Eager loading, load everything

//Join column
@JoinColumn(name = "column_name")

//Join table
@JoinTable(name = "table_name", joinColumns = @JoinColumn(name = "column_name"), inverseJoinColumns = @JoinColumn(name = "column_name"))

//Temporal
@Temporal(TemporalType.DATE) //Only date

@Temporal(TemporalType.TIME) //Only time

@Temporal(TemporalType.TIMESTAMP) //Date and time

//Default value for column with exmaple
@Column(name = "column_name", columnDefinition = "boolean default false")
//another example
@Column(name = "column_name", columnDefinition = "int default 0")
```

## Repository:

```java	
//Put this before the class
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository

//add the extend
extends JpaRepository<Object, Long>
```

### Query methods:

- `findBy<Property>`
- `findFirstBy<Property>`
- `findTopBy<Property>`
- `findBy<Property1>And<Property2>`
- `findBy<Property1>Or<Property2>`
- `findBy<Property>OrderBy<Property>Desc`
- `findFirst3By<Property>`
- `existsBy<Property>`
- `countBy<Property>`
- `deleteBy<Property>`
- `findDistinctBy<Property>`

### Query annotation:

```java
@Query("SELECT c FROM Child c WHERE c.property = :property")
@Query("SELECT c FROM Child c JOIN c.parent p WHERE p.property = :property")
@Query("SELECT c FROM Child c WHERE c.property = :property ORDER BY c.property DESC")
@Query("SELECT c FROM Child c WHERE c.property = :property GROUP BY c.property")
@Query("SELECT c FROM Child c WHERE c.property = :property HAVING COUNT(c) > 1")
@Query("SELECT c FROM Child c WHERE c.property = :property ORDER BY c.property DESC LIMIT 3")
```

## Service:

```java
//dont forget to check the table is null before adding etc
//schedule task every 5 minutes
@Scheduled(fixedRate = 300000)
public void scheduleTaskWithFixedRate() {
    //do something
}
//Scheduler with * * * * * * (second [0-60], minute [0-60], hour [0-23], day of month [1-31], month [1-12], day of week [0-7]) week starts with sunday
@Scheduled(cron = "0 * * * * *")
public void scheduleTaskWithCronExpression() {
    //do something
}
//add the @EnableScheduling in the main class
```

## Aspect:

```java
//Add the @@EnableAspectJAutoProxy in the main class

//This at the start

import lombok.extern.slf4j.Slf4j;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.JoinPoint;

@Aspect
@Slf4j

//Before execution (before returning)
@Before("execution(* com.example.demo.*.*(..))")
public void beforeExecution(JoinPoint joinPoint) {
    //do something
    //get method name
    String name = joinPoint.getSignature().getName();
}

//After execution (after returning)
@AfterReturning(pointcut = "execution(* com.example.demo.*.*(..))", returning = "result")
public void afterExecution(JoinPoint joinPoint, Object result) {
    //do something
}

//Around execution (before and after) and continue work
@Around("execution(* com.example.demo.*.*(..))")
public void aroundExecution(ProceedingJoinPoint joinPoint) {
    //do something before
    joinPoint.proceed();
    //do something after
}

//Exception handling
@AfterThrowing(pointcut = "execution(* com.example.demo.*.*(..))", throwing = "exception")
public void afterThrowing(JoinPoint joinPoint, Throwable exception) {
    //do something
}
```

## Controller:

```java
//GetMapping
@GetMapping("/path")

//PostMapping
@PostMapping("/path")

//PutMapping
@PutMapping("/path")

//DeleteMapping
@DeleteMapping("/path")

//PathVariable
@GetMapping("/path/{id}")
public void methodName(@PathVariable Long id) {
    //do something
}

//RequestBody
@PostMapping("/path")
public void methodName(@RequestBody Object object) {
    //do something
}

//RequestParam
@GetMapping("/path")
public void methodName(@RequestParam String param) {
    //do something
}
