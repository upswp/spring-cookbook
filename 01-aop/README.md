# AOP (Aspect-Oriented Programming)

## 학습 의도

실무에서 비즈니스 로직을 작성하다 보면 모든 메서드에 동일한 로깅 코드를 복사하고, 실행 시간 측정 코드를 중복해서 작성하며, 예외 처리와 보안 체크 코드를 반복적으로 추가하게 됩니다. 결과적으로 실제 비즈니스 로직은 2-3줄인데, 이를 둘러싼 공통 관심사 코드가 25줄 이상이 되는 경우가 흔합니다.

이 예제는 AOP를 사용하지 않았을 때와 사용했을 때를 직접 비교함으로써, AOP의 필요성을 체감하게 합니다. 단순히 AOP의 개념을 설명하는 것이 아니라, 실제 코드를 통해 83%의 코드 감소를 확인하고, 가독성과 유지보수성이 얼마나 향상되는지를 경험하게 됩니다.

## 학습 목표

### AOP의 필요성 체감
AOP 없이 코딩하면 중복 코드로 인한 고통을 겪게 되고, AOP를 사용하면 깔끔한 코드를 유지할 수 있다는 것을 직접 확인합니다. "왜 AOP가 필요한가"에 대한 명확한 답을 얻게 됩니다.

### 코드 품질 개선
관심사의 분리(Separation of Concerns) 원칙을 적용하고, DRY(Don't Repeat Yourself) 원칙을 실천하며, 유지보수성을 향상시키는 방법을 익힙니다. 로깅 형식을 변경하거나 새로운 공통 관심사를 추가할 때, 한 곳만 수정하면 되는 구조의 가치를 이해합니다.

### 실무 적용 능력
언제 AOP를 써야 하는지 판단할 수 있고, Aspect를 효과적으로 설계할 수 있으며, 포인트컷 표현식을 작성할 수 있고, 프록시 패턴의 동작 원리를 이해하게 됩니다.

## 실전 시나리오

사용자 관리 서비스를 개발한다고 가정합니다. 모든 메서드의 실행 시간을 측정해야 하고, 모든 메서드의 입출력을 로깅해야 하며, 예외 발생 시 상세한 로그를 남겨야 하고, 관리자 권한을 체크해야 합니다.

AOP를 사용하지 않으면 getUser 메서드는 30줄이 되는데, 그 중 비즈니스 로직은 단 3줄뿐입니다. 나머지 27줄은 실행 시간 측정, 메서드 로깅, 보안 체크, 반환값 로깅, 예외 처리로 채워집니다. createUser, deleteUser 메서드도 마찬가지로 동일한 패턴이 반복됩니다.

AOP를 사용하면 UserService는 5줄로 줄어들고, 순수한 비즈니스 로직만 남습니다. 로깅, 성능 측정, 예외 처리, 보안 체크는 모두 Aspect가 자동으로 처리합니다.

## 예제 구조

이 예제는 크게 두 부분으로 구성됩니다. without-aop 디렉토리에는 AOP를 사용하지 않은 버전이 있고, UserService.java 파일이 150줄로 중복 코드가 많습니다. with-aop 디렉토리에는 AOP를 사용한 버전이 있으며, UserService.java는 30줄로 깔끔하게 정리되어 있고, aspect 패키지 아래에 PerformanceAspect, LoggingAspect, ExceptionAspect, SecurityAspect가 분리되어 있습니다.

COMPARISON.md 파일에서는 두 버전을 상세히 비교합니다.

## 기대 효과

### "왜 AOP인가"에 대한 완벽한 이해

면접에서 "AOP가 무엇인가요?"라는 질문을 받았을 때, "횡단 관심사를 처리하는 것"이라고만 답하는 것이 아니라, "비즈니스 로직에 반복되는 공통 기능을 분리하여 코드 중복을 제거하고 유지보수성을 높이는 기법"이라고 설명할 수 있습니다. 그리고 실제로 150줄 코드를 30줄로 줄인 경험을 언급할 수 있습니다.

### 코드 품질 감각

중복 코드의 냄새를 즉시 감지할 수 있고, "이 부분은 Aspect로 분리해야겠다"는 판단을 내릴 수 있습니다. 깔끔한 코드를 작성하는 습관이 자연스럽게 형성됩니다.

### 실무 활용 능력

프로젝트에 즉시 적용할 수 있는 능력이 생깁니다. 로깅, 트랜잭션, 보안 등 실전 케이스를 다루고, 포인트컷 표현식을 자유자재로 활용할 수 있게 됩니다.

### Spring 생태계 이해

@Transactional이 AOP로 구현되어 있다는 것을 이해하고, @Cacheable이 AOP로 동작한다는 것을 파악하며, Spring Security의 AOP 기반 인가 메커니즘을 이해하게 됩니다.

## 정량적 효과

코드 라인 수를 비교하면, AOP 없이는 150줄이 필요했던 것이 AOP를 사용하면 30줄로 줄어 80% 감소합니다. 중복 코드는 60줄에서 0줄로 완전히 제거됩니다. 유지보수 시 수정해야 하는 곳도 3곳에서 1곳으로 67% 감소합니다. 가독성은 크게 향상되고, 테스트 작성이 훨씬 쉬워집니다.

## 핵심 개념

### Aspect
횡단 관심사를 모듈화한 것입니다. 로깅, 보안, 트랜잭션 관리 같은 공통 기능을 하나의 모듈로 묶어서 관리합니다.

### Join Point
Aspect가 적용될 수 있는 지점을 의미합니다. 메서드 실행, 필드 접근, 객체 생성 등이 Join Point가 될 수 있습니다.

### Advice
특정 Join Point에서 실제로 실행되는 코드입니다. @Before는 메서드 실행 전에, @After는 메서드 실행 후에, @AfterReturning은 메서드가 정상 종료된 후에, @AfterThrowing은 예외가 발생했을 때, @Around는 메서드 실행 전후로 실행됩니다. @Around가 가장 강력하지만, 필요한 경우에만 사용해야 합니다.

### Pointcut
Advice가 적용될 Join Point를 선택하는 표현식입니다. execution 표현식을 사용하여 특정 패턴의 메서드에만 Aspect를 적용할 수 있습니다.

### Weaving
Aspect를 대상 객체에 적용하는 과정입니다. Spring AOP는 런타임 위빙을 사용합니다.

## 구현 예제

### 실행 시간 측정
PerformanceAspect는 @Around 어드바이스를 사용하여 메서드 실행 시간을 측정합니다. execution 포인트컷으로 모든 서비스 메서드에 자동으로 적용됩니다.

### 메서드 로깅
LoggingAspect는 @Before로 메서드 호출 시 파라미터를 로깅하고, @AfterReturning으로 메서드 반환 시 결과를 로깅합니다.

### 예외 처리
ExceptionAspect는 @AfterThrowing을 사용하여 예외 발생 시 상세한 로그를 남기고, 필요하다면 관리자에게 알림을 전송하거나 외부 모니터링 시스템에 전송할 수 있습니다.

### 보안 체크
SecurityAspect는 @Before를 사용하여 메서드 실행 전에 권한을 확인합니다. 권한이 없으면 SecurityException을 발생시킵니다.

## 포인트컷 표현식

기본 패턴은 `execution(modifiers-pattern? return-type-pattern declaring-type-pattern? method-name-pattern(param-pattern) throws-pattern?)`입니다.

모든 public 메서드에 적용하려면 `execution(public * *(..))`을 사용하고, 특정 패키지의 모든 메서드에 적용하려면 `execution(* com.example.service.*.*(..))`을 사용합니다. 특정 메서드명 패턴에 적용하려면 `execution(* save*(..))`처럼 작성하고, 특정 애노테이션이 붙은 메서드에 적용하려면 `@annotation(org.springframework.transaction.annotation.Transactional)`을 사용합니다.

## 실행 방법

Spring Boot는 AOP를 자동으로 활성화하지만, 명시적으로 표시하려면 @EnableAspectJAutoProxy를 사용합니다. build.gradle에 spring-boot-starter-aop 의존성을 추가하면 됩니다.

without-aop 프로젝트를 실행하면 각 메서드마다 25줄 이상의 중복 코드를 확인할 수 있고, with-aop 프로젝트를 실행하면 깔끔한 비즈니스 로직과 자동으로 적용되는 Aspect를 확인할 수 있습니다.

## 프록시 vs CGLIB

JDK Dynamic Proxy는 인터페이스 기반으로 동작하며, 인터페이스가 있는 경우에 사용됩니다. Java 표준 기술입니다.

CGLIB Proxy는 클래스 기반으로 동작하며, 인터페이스 없이도 사용할 수 있습니다. Spring Boot 2.0부터는 CGLIB가 기본값입니다.

application.yml에서 spring.aop.proxy-target-class를 true로 설정하면 CGLIB를 사용합니다.

## Best Practice

Aspect는 최소한으로 유지해야 합니다. 너무 많은 Aspect는 디버깅을 어렵게 만듭니다.

포인트컷은 재사용하는 것이 좋습니다. CommonPointcuts 클래스를 만들어 자주 사용하는 포인트컷을 정의해두면 편리합니다.

@Around는 강력하지만 오버헤드가 있으므로 필요한 경우에만 사용합니다.

Aspect 클래스명에 목적을 명확히 표현합니다. PerformanceLoggingAspect, SecurityAspect, TransactionAspect처럼 이름만 봐도 역할을 알 수 있어야 합니다.

## 디버깅 팁

AOP가 적용되지 않을 때는 먼저 @EnableAspectJAutoProxy가 설정되어 있는지 확인하고, 포인트컷 표현식이 올바른지 검증하며, 프록시 타입(인터페이스 vs 클래스)을 확인합니다. 특히 self-invocation 문제를 주의해야 합니다. 같은 클래스 내에서 메서드를 호출하면 AOP가 적용되지 않습니다.

## 참고 자료

더 자세한 내용은 Spring AOP 공식 문서와 AspectJ 문서를 참고하시기 바랍니다. COMPARISON.md 파일에서 without-aop와 with-aop의 상세한 비교를 확인할 수 있습니다.

---

*작성일: 2026-01-30*
