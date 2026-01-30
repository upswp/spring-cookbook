# 🎯 AOP (Aspect-Oriented Programming)

> Spring AOP 실전 구현 가이드

## 📋 개요

AOP는 횡단 관심사(Cross-Cutting Concerns)를 모듈화하는 프로그래밍 패러다임입니다.
로깅, 보안, 트랜잭션 관리 등 비즈니스 로직과 분리된 공통 기능을 효과적으로 처리할 수 있습니다.

---

## 🎓 핵심 개념

### 1. Aspect (애스펙트)
횡단 관심사를 모듈화한 것

### 2. Join Point (조인 포인트)
Aspect가 적용될 수 있는 지점 (메서드 실행, 필드 접근 등)

### 3. Advice (어드바이스)
특정 Join Point에서 실행되는 코드
- **@Before**: 메서드 실행 전
- **@After**: 메서드 실행 후
- **@AfterReturning**: 메서드 정상 종료 후
- **@AfterThrowing**: 예외 발생 시
- **@Around**: 메서드 실행 전후 (가장 강력)

### 4. Pointcut (포인트컷)
Advice가 적용될 Join Point를 선택하는 표현식

### 5. Weaving (위빙)
Aspect를 대상 객체에 적용하는 과정

---

## 🛠️ 구현 예제

### 예제 1: 실행 시간 측정 (Performance Logging)

```java
@Aspect
@Component
public class PerformanceAspect {

    @Around("execution(* com.example.service.*.*(..))")
    public Object measureExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long startTime = System.currentTimeMillis();

        Object result = joinPoint.proceed();

        long endTime = System.currentTimeMillis();
        long executionTime = endTime - startTime;

        System.out.println(joinPoint.getSignature() + " 실행 시간: " + executionTime + "ms");

        return result;
    }
}
```

### 예제 2: 메서드 로깅

```java
@Aspect
@Component
public class LoggingAspect {

    @Before("execution(* com.example.service.*.*(..))")
    public void logBefore(JoinPoint joinPoint) {
        System.out.println("메서드 호출: " + joinPoint.getSignature().getName());
        System.out.println("파라미터: " + Arrays.toString(joinPoint.getArgs()));
    }

    @AfterReturning(
        pointcut = "execution(* com.example.service.*.*(..))",
        returning = "result"
    )
    public void logAfterReturning(JoinPoint joinPoint, Object result) {
        System.out.println("메서드 반환: " + joinPoint.getSignature().getName());
        System.out.println("결과: " + result);
    }
}
```

### 예제 3: 예외 처리

```java
@Aspect
@Component
public class ExceptionHandlingAspect {

    @AfterThrowing(
        pointcut = "execution(* com.example.service.*.*(..))",
        throwing = "ex"
    )
    public void logException(JoinPoint joinPoint, Exception ex) {
        System.err.println("예외 발생: " + joinPoint.getSignature().getName());
        System.err.println("예외 메시지: " + ex.getMessage());

        // 예외 로깅, 알림 전송 등 추가 처리
    }
}
```

---

## 🎯 포인트컷 표현식

### 기본 패턴

```java
execution(modifiers-pattern? return-type-pattern declaring-type-pattern? method-name-pattern(param-pattern) throws-pattern?)
```

### 예제

```java
// 모든 public 메서드
@Pointcut("execution(public * *(..))")

// 특정 패키지의 모든 메서드
@Pointcut("execution(* com.example.service.*.*(..))")

// 특정 클래스의 모든 메서드
@Pointcut("execution(* com.example.service.UserService.*(..))")

// 특정 메서드명 패턴
@Pointcut("execution(* save*(..))")

// 특정 파라미터 타입
@Pointcut("execution(* com.example.service.*.*(String, ..))")

// @Transactional 애노테이션이 붙은 메서드
@Pointcut("@annotation(org.springframework.transaction.annotation.Transactional)")
```

---

## 🚀 실행 방법

### 1. 의존성 추가 (build.gradle)

```gradle
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-aop'
}
```

### 2. AOP 활성화

Spring Boot는 자동으로 AOP를 활성화하지만, 명시적으로 활성화하려면:

```java
@SpringBootApplication
@EnableAspectJAutoProxy
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

### 3. 애플리케이션 실행

```bash
./gradlew bootRun
```

---

## 📊 프록시 vs CGLIB

### JDK Dynamic Proxy (기본)
- 인터페이스 기반
- 인터페이스가 있는 경우 사용
- Java 표준 기술

### CGLIB Proxy
- 클래스 기반
- 인터페이스 없이도 사용 가능
- Spring Boot 기본값 (Spring Boot 2.0+)

### 설정

```yaml
# application.yml
spring:
  aop:
    proxy-target-class: true  # CGLIB 사용 (기본값)
```

---

## 💡 Best Practice

### 1. Aspect는 최소한으로
너무 많은 Aspect는 디버깅을 어렵게 만듭니다.

### 2. 포인트컷 재사용
```java
@Aspect
@Component
public class CommonPointcuts {
    @Pointcut("execution(* com.example.service.*.*(..))")
    public void serviceLayer() {}

    @Pointcut("execution(* com.example.repository.*.*(..))")
    public void repositoryLayer() {}
}
```

### 3. 성능 고려
@Around는 강력하지만 오버헤드가 있습니다. 필요한 경우에만 사용하세요.

### 4. 명확한 네이밍
Aspect 클래스명에 목적을 명확히 표현하세요.
- `PerformanceLoggingAspect`
- `SecurityAspect`
- `TransactionAspect`

---

## 🔍 디버깅 팁

### AOP가 적용되지 않을 때

1. **@EnableAspectJAutoProxy 확인**
2. **포인트컷 표현식 검증**
3. **프록시 타입 확인** (인터페이스 vs 클래스)
4. **self-invocation 주의** (같은 클래스 내 메서드 호출은 AOP 미적용)

---

## 📚 참고 자료

- [Spring AOP 공식 문서](https://docs.spring.io/spring-framework/reference/core/aop.html)
- [AspectJ 문서](https://www.eclipse.org/aspectj/doc/released/progguide/index.html)

---

**다음 주제**: [02-di-ioc](../02-di-ioc)
