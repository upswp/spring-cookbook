# Spring Deep Dive

## 프로젝트 개요

이 저장소는 Spring Framework의 핵심 개념들을 단계적으로 구현하면서 그 본질을 이해하는 것을 목표로 합니다. 단순히 Spring의 기능을 사용하는 방법을 배우는 것이 아니라, 왜 이런 방식이 필요한지, 내부적으로 어떻게 동작하는지를 직접 구현해보면서 깨닫게 됩니다.

각 주제는 "Spring 없이 직접 구현" 단계부터 시작하여, 그 과정에서 겪는 어려움을 통해 Spring의 가치를 이해하게 됩니다. 이론적인 설명보다는 실제 동작하는 코드를 통해 학습하며, 모든 예제에는 테스트 코드가 포함되어 있습니다.

## 학습 목표

### Spring의 본질 이해
@Autowired, @Component 같은 애노테이션을 단순히 사용하는 것을 넘어서, 왜 의존성 주입이 필요한지, DI 컨테이너는 어떻게 구현되는지, AOP는 어떤 원리로 동작하는지를 파악합니다.

### 내부 동작 원리 파악
Spring이 마법처럼 느껴지지 않도록, 리플렉션을 통한 자동 주입, 프록시 패턴을 활용한 AOP, Bean Lifecycle 관리 등의 내부 메커니즘을 직접 구현해봅니다.

### 실무 적용 능력 향상
순환 참조 문제 해결, 적절한 주입 방식 선택, Aspect 설계, 포인트컷 표현식 작성 등 실무에서 마주치는 문제들을 해결할 수 있는 능력을 기릅니다.

## 주제별 구성

### 1. AOP (Aspect-Oriented Programming)

**학습 내용:**
- AOP 기본 개념 및 구현
- @Around, @Before, @After, @AfterReturning, @AfterThrowing 어드바이스
- 포인트컷 표현식 작성
- 프록시 패턴 vs CGLIB
- 실전 예제: 로깅, 성능 측정, 예외 처리, 보안 체크

**구성:**
- without-aop: AOP 없이 구현 (중복 코드 많음, 150줄)
- with-aop: AOP 사용 (깔끔한 코드, 30줄)
- 코드 83% 감소 효과 확인

### 2. DI & IoC (의존성 주입과 제어의 역전)

**학습 내용:**
- 생성자 주입 vs Setter 주입 vs 필드 주입
- @Autowired, @Qualifier, @Primary 사용법
- Bean 생명주기 및 스코프
- 순환 참조 문제 해결
- DI 컨테이너 직접 구현

**구성:**
- Level 1: Manual DI (수동 의존성 주입)
- Level 2: Simple Container (간단한 DI 컨테이너)
- Level 3: Reflection DI (리플렉션 기반 자동 주입)
- Level 4: Spring DI (Spring Framework 사용)

순수 Kotlin으로 직접 구현하면서 Spring의 내부 동작을 이해합니다.

### 3. Transaction Management

트랜잭션 관리의 기본 원리와 Spring의 @Transactional 동작 방식을 학습합니다.

- @Transactional 동작 원리
- 전파 속성 (Propagation)
- 격리 수준 (Isolation Level)
- 트랜잭션 롤백 전략
- 실전 예제: 복잡한 비즈니스 로직의 트랜잭션 처리

### 4. Spring Data JPA

JPA와 Spring Data JPA의 핵심 개념을 학습합니다.

- JPA Repository 활용
- QueryDSL 통합
- N+1 문제 해결
- 페이징 및 정렬
- 복잡한 쿼리 최적화

### 5. Spring MVC

웹 애플리케이션 개발의 핵심 개념을 다룹니다.

- REST API 설계
- 요청/응답 처리
- 예외 처리 (@ControllerAdvice)
- Validation
- RESTful API 구현

### 6. Spring Security

보안 구현의 기본부터 고급 주제까지 다룹니다.

- 인증 (Authentication)
- 인가 (Authorization)
- JWT 토큰 기반 인증
- OAuth2 소셜 로그인
- 보안 설정 및 커스터마이징

### 7. Spring Boot Features

Spring Boot의 편의 기능들을 학습합니다.

- Auto Configuration
- 외부 설정 관리 (application.yml)
- Profile 활용
- Actuator 모니터링
- 운영 환경 설정

### 8. Testing

효과적인 테스트 전략을 학습합니다.

- 단위 테스트 (JUnit, Mockito)
- 통합 테스트 (@SpringBootTest)
- Slice 테스트 (@WebMvcTest, @DataJpaTest)
- TestContainers
- 테스트 가능한 코드 작성

## 기술 스택

- Java 17+
- Kotlin (일부 예제)
- Spring Boot 3.x
- Spring Framework 6.x
- Gradle
- H2 Database (테스트용)
- PostgreSQL (실전용)
- JUnit 5, Mockito, AssertJ

## 빠른 시작

각 예제는 독립적으로 실행 가능합니다.

```bash
# 원하는 주제로 이동
cd 01-aop/with-aop

# Gradle로 빌드
./gradlew build

# 애플리케이션 실행
./gradlew bootRun

# 테스트 실행
./gradlew test
```

## 학습 순서 (권장)

1. **02-di-ioc** - Spring의 핵심 개념 (필수)
2. **01-aop** - 횡단 관심사 처리
3. **03-transaction** - 데이터 정합성 보장
4. **04-spring-data-jpa** - 데이터베이스 연동
5. **05-spring-mvc** - 웹 애플리케이션 개발
6. **06-spring-security** - 보안 구현
7. **07-spring-boot** - 운영 환경 구성
8. **08-testing** - 품질 보장

## 프로젝트 특징

### 실무 중심
이론만이 아닌 실제 프로젝트에서 사용할 수 있는 코드를 제공합니다.

### 단계적 학습
쉬운 것부터 복잡한 것으로, 직접 구현해보면서 Spring의 가치를 이해합니다.

### 완전한 테스트
모든 예제에 단위 테스트 및 통합 테스트가 포함되어 있습니다.

### 상세한 설명
코드에 상세한 주석을 포함하고, 각 주제별로 학습 의도와 목표를 명확히 합니다.

### Best Practice
Spring 공식 문서 및 커뮤니티의 Best Practice를 반영합니다.

## 참고 자료

- [Spring 공식 문서](https://spring.io/projects/spring-framework)
- [Spring Boot 공식 문서](https://spring.io/projects/spring-boot)
- [Baeldung Spring Tutorials](https://www.baeldung.com/spring-tutorial)

## 라이선스

MIT License - 자유롭게 사용하세요.

## Contact

- GitHub: [@upswp](https://github.com/upswp)
- Repository: [spring-deep-dive](https://github.com/upswp/spring-deep-dive)

---

*작성일: 2026-01-30*
*목적: Spring Framework의 본질을 이해하고, 내부 동작 원리를 파악한다*
