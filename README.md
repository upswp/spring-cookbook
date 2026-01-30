# 🍳 Spring Cookbook

> Spring Framework 실전 구현 레시피 모음

Spring의 다양한 기능을 실제로 구현하고 테스트해보는 Repository입니다.
각 주제별로 독립적인 예제 프로젝트가 포함되어 있으며, 실무에서 바로 적용할 수 있는 코드를 제공합니다.

---

## 📚 목차

### 1. [AOP (Aspect-Oriented Programming)](./01-aop)
- ✅ AOP 기본 개념 및 구현
- ✅ @Around, @Before, @After 어드바이스
- ✅ 포인트컷 표현식
- ✅ 프록시 vs CGLIB
- ✅ 실전 예제: 로깅, 성능 측정, 트랜잭션

### 2. [DI & IoC (의존성 주입)](./02-di-ioc)
- 생성자 주입 vs Setter 주입 vs 필드 주입
- @Autowired, @Qualifier, @Primary
- Bean 생명주기 및 스코프
- 순환 참조 해결
- 실전 예제: 다양한 주입 방식 비교

### 3. [Transaction Management](./03-transaction)
- @Transactional 동작 원리
- 전파 속성 (Propagation)
- 격리 수준 (Isolation Level)
- 트랜잭션 롤백 전략
- 실전 예제: 복잡한 비즈니스 로직 트랜잭션 처리

### 4. [Spring Data JPA](./04-spring-data-jpa)
- JPA Repository 활용
- QueryDSL 통합
- N+1 문제 해결
- 페이징 및 정렬
- 실전 예제: 복잡한 쿼리 최적화

### 5. [Spring MVC](./05-spring-mvc)
- REST API 설계
- 요청/응답 처리
- 예외 처리 (@ControllerAdvice)
- Validation
- 실전 예제: RESTful API 구현

### 6. [Spring Security](./06-spring-security)
- 인증 (Authentication)
- 인가 (Authorization)
- JWT 토큰 기반 인증
- OAuth2 소셜 로그인
- 실전 예제: 보안 설정 및 커스터마이징

### 7. [Spring Boot Features](./07-spring-boot)
- Auto Configuration
- 외부 설정 관리 (application.yml)
- Profile 활용
- Actuator 모니터링
- 실전 예제: 운영 환경 설정

### 8. [Testing](./08-testing)
- 단위 테스트 (JUnit, Mockito)
- 통합 테스트 (@SpringBootTest)
- Slice 테스트 (@WebMvcTest, @DataJpaTest)
- TestContainers
- 실전 예제: 효과적인 테스트 전략

---

## 🛠️ 기술 스택

- **Java**: 17+
- **Spring Boot**: 3.x
- **Spring Framework**: 6.x
- **Build Tool**: Gradle
- **Database**: H2 (테스트용), PostgreSQL (실전용)
- **Testing**: JUnit 5, Mockito, AssertJ

---

## 🚀 빠른 시작

### 각 예제 실행 방법

```bash
# 원하는 주제 디렉토리로 이동
cd 01-aop

# Gradle로 빌드
./gradlew build

# 애플리케이션 실행
./gradlew bootRun

# 테스트 실행
./gradlew test
```

---

## 📖 사용 방법

### 1. 주제별로 독립적으로 학습
각 디렉토리는 독립적인 Spring Boot 프로젝트입니다.

```bash
cd 01-aop
```

### 2. README 먼저 읽기
각 주제별로 상세한 README가 있습니다.

```
01-aop/
├── README.md          ← 개념 설명 및 사용법
├── src/
│   ├── main/
│   │   └── java/     ← 실제 구현 코드
│   └── test/
│       └── java/     ← 테스트 코드
└── build.gradle
```

### 3. 코드 실행 및 테스트
직접 실행해보고 코드를 수정하면서 학습하세요.

---

## 🎯 학습 순서 (추천)

1. **02-di-ioc** - Spring의 핵심 개념 (필수)
2. **01-aop** - 횡단 관심사 처리
3. **03-transaction** - 데이터 정합성 보장
4. **04-spring-data-jpa** - 데이터베이스 연동
5. **05-spring-mvc** - 웹 애플리케이션 개발
6. **06-spring-security** - 보안 구현
7. **07-spring-boot** - 운영 환경 구성
8. **08-testing** - 품질 보장

---

## 📌 특징

### ✅ 실무 중심
이론만이 아닌 실제 프로젝트에서 사용할 수 있는 코드

### ✅ 테스트 포함
모든 예제에 단위 테스트 및 통합 테스트 포함

### ✅ 상세한 주석
코드에 상세한 주석으로 이해를 도움

### ✅ Best Practice
Spring 공식 문서 및 커뮤니티 Best Practice 반영

---

## 🔗 참고 자료

- [Spring 공식 문서](https://spring.io/projects/spring-framework)
- [Spring Boot 공식 문서](https://spring.io/projects/spring-boot)
- [Baeldung Spring Tutorials](https://www.baeldung.com/spring-tutorial)

---

## 🤝 기여

이슈 및 Pull Request 환영합니다!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📝 라이선스

MIT License - 자유롭게 사용하세요.

---

## 📧 Contact

- GitHub: [@upswp](https://github.com/upswp)

---

**⭐ 도움이 되었다면 Star를 눌러주세요!**

---

*Last Updated: 2026-01-30*
