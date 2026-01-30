# IoC & DI (Inversion of Control & Dependency Injection)

## 학습 의도

이 예제는 Spring Framework의 IoC와 DI를 단계적으로 구현하면서 그 본질을 이해하는 것을 목표로 합니다. 대부분의 개발자들은 @Autowired, @Component 같은 애노테이션을 사용하지만, 왜 의존성 주입이 필요한지, Spring이 내부적으로 어떻게 동작하는지에 대해서는 명확히 알지 못하는 경우가 많습니다.

이 예제에서는 순수 Kotlin으로 직접 DI 컨테이너를 구현해보고, 그 과정에서 겪는 어려움을 통해 Spring의 가치를 이해하게 됩니다. 단순히 Spring을 사용하는 방법을 배우는 것이 아니라, 왜 이런 방식이 필요한지를 체득하는 것이 핵심입니다.

## 학습 목표

### IoC/DI의 본질 이해
의존성 역전 원칙(Dependency Inversion Principle)과 제어의 역전(Inversion of Control)이 무엇인지, 그리고 왜 객체를 직접 생성하지 않고 외부에서 주입받아야 하는지를 이해합니다. 이론적인 설명보다는 실제 코드를 작성하면서 강한 결합이 가져오는 문제점을 직접 경험하게 됩니다.

### Spring의 내부 동작 원리
DI 컨테이너가 어떻게 구현되는지, Bean은 어떻게 관리되는지, 리플렉션을 통한 자동 주입이 어떤 원리로 동작하는지를 파악합니다. ApplicationContext가 하는 역할을 직접 구현해봄으로써 Spring의 핵심 메커니즘을 이해할 수 있습니다.

### 실무 적용 능력
언제 생성자 주입을 써야 하는지, Setter 주입과 필드 주입의 차이가 무엇인지, 순환 참조 문제는 어떻게 해결하는지 등 실무에서 마주치는 문제들을 해결할 수 있는 능력을 기릅니다.

## 단계별 구성

### Level 1: Manual DI (수동 의존성 주입)

첫 번째 단계에서는 의존성 주입 없이 코드를 작성합니다. 모든 객체를 직접 new 키워드로 생성하고, 의존 관계를 하드코딩합니다. 이 과정에서 강한 결합(Tight Coupling)이 가져오는 문제점들을 직접 체험하게 됩니다.

객체를 직접 생성하면 교체가 어렵고, 테스트 시 Mock 객체를 주입할 수 없으며, 설정을 변경하려면 코드를 직접 수정해야 합니다. 또한 같은 구현체가 여러 곳에서 생성되어 재사용성이 떨어집니다.

```kotlin
class UserService {
    private val repository = UserRepositoryImpl()
    private val notificationService = EmailService()

    fun registerUser(name: String) {
        // 만약 SMS로 알림을 바꾸려면 코드를 직접 수정해야 함
    }
}
```

이 방식의 가장 큰 문제는 UserService가 구체적인 구현체에 직접 의존한다는 것입니다. 인터페이스가 아닌 구현체에 의존하기 때문에 다른 구현체로 교체하기가 매우 어렵습니다.

### Level 2: Simple Container (간단한 DI 컨테이너)

두 번째 단계에서는 객체 생성과 관리를 중앙 집중화하는 간단한 컨테이너를 직접 구현합니다. 이제 객체들을 컨테이너에 등록하고, 필요할 때 꺼내서 사용하는 방식으로 변경됩니다.

```kotlin
class DIContainer {
    private val instances = mutableMapOf<Class<*>, Any>()

    fun <T : Any> register(clazz: Class<T>, instance: T) {
        instances[clazz] = instance
    }

    fun <T : Any> get(clazz: Class<T>): T {
        return instances[clazz] as T
    }
}
```

이 방식은 객체 생성을 한 곳에서 관리할 수 있게 해줍니다. 인터페이스 기반으로 프로그래밍할 수 있고, 테스트 시 Mock 객체를 주입하는 것도 가능해집니다. 하지만 여전히 모든 객체를 수동으로 등록해야 하고, 생성자 주입도 직접 처리해야 하는 불편함이 있습니다.

### Level 3: Reflection DI (리플렉션 기반 DI)

세 번째 단계에서는 리플렉션을 활용하여 Spring과 유사한 방식의 DI 컨테이너를 구현합니다. 애노테이션을 정의하고, 클래스를 자동으로 스캔하며, 생성자 파라미터를 분석하여 의존성을 자동으로 주입합니다.

```kotlin
@Component
class UserService @Inject constructor(
    private val userRepository: UserRepository,
    private val notificationService: NotificationService
) {
    // 컨테이너가 자동으로 의존성을 주입
}
```

이 단계에서는 애노테이션 기반의 자동 주입, 컴포넌트 스캔, Singleton 관리 등이 구현됩니다. 하지만 여전히 Lazy Loading, AOP 지원, Profile 관리, Bean Lifecycle 관리, 순환 참조 해결 등에서는 한계가 있습니다.

### Level 4: Spring DI

마지막 단계에서는 Spring Framework를 사용합니다. 앞의 세 단계에서 직접 구현하면서 겪었던 모든 어려움을 Spring이 어떻게 해결하는지 확인합니다.

```kotlin
@Service
class UserService(
    private val userRepository: UserRepository,
    private val notificationService: NotificationService
) {
    // 비즈니스 로직에만 집중
}
```

Spring은 완전한 자동화, 다양한 주입 방식 지원, 완벽한 Bean Lifecycle 관리, Lazy Loading과 Prototype Scope, AOP 통합, Profile 기반 설정, 순환 참조 자동 감지, 테스트 지원 등 모든 것을 제공합니다.

## 학습 효과

### 본질적 이해

의존성 주입이 왜 필요한지를 이론이 아닌 실제 코드를 통해 이해하게 됩니다. Spring이 어떻게 동작하는지 막연하게 아는 것이 아니라, 직접 구현해봤기 때문에 내부 메커니즘을 정확히 파악할 수 있습니다.

### 면접 대응 능력

"의존성 주입이 무엇인가요?"라는 질문에 단순히 "@Autowired 쓰면 되는 것"이라고 답하는 것이 아니라, 객체 간 결합도를 낮추기 위한 설계 패턴이며, 직접 객체를 생성하는 대신 외부에서 주입받아 유연성을 높이는 방식이라고 명확히 설명할 수 있습니다.

"DI 컨테이너는 어떻게 동작하나요?"라는 질문에는 리플렉션을 통해 클래스 정보를 읽고, 생성자의 파라미터 타입을 파악하여 자동으로 의존성을 주입한다고 답할 수 있습니다. 직접 구현해본 경험을 바탕으로 더 깊이 있는 대화가 가능합니다.

### 실무 능력 향상

순환 참조 문제가 발생했을 때 원인을 빠르게 파악할 수 있고, Bean Lifecycle을 이해하여 초기화 로직을 적절한 위치에 배치할 수 있습니다. 테스트 가능한 코드를 작성하는 능력도 자연스럽게 향상됩니다.

## 비교 분석

수동 방식에서는 코드량이 많고 결합도가 강하며, 자동화가 전혀 없고 테스트가 어려우며 유지보수가 힘듭니다. 간단한 컨테이너로 개선하면 코드량이 줄고 결합도가 약해지지만, 자동화는 여전히 부분적이고 Lifecycle 관리가 부족합니다.

리플렉션 기반으로 발전시키면 코드량이 더 줄고 자동화가 많아지지만, AOP 통합이나 Profile 지원은 없습니다. Spring을 사용하면 코드량이 최소화되고, 완전한 자동화와 모든 기능이 제공됩니다.

## 권장 학습 순서

첫째 날에는 Manual DI와 Simple Container를 실행하며 불편함을 체감하고, 컨테이너의 가치를 이해합니다. 코드를 비교하며 차이점을 분석합니다.

둘째 날에는 Reflection DI 코드를 읽고 Reflection API를 이해하며, 직접 수정해보면서 실험합니다.

셋째 날에는 Spring DI를 실행하고 Level 3와 비교하며, Spring의 추가 기능들을 탐구합니다.

마지막 날에는 COMPARISON.md를 읽고 각 단계별 장단점을 정리하며, 면접 질문에 대한 답변을 준비합니다.

## 자주 묻는 질문

왜 순수 Kotlin으로 구현하는가에 대한 질문이 있습니다. Java로 하면 보일러플레이트 코드가 너무 많아 핵심 개념이 흐려지기 때문입니다. Kotlin의 간결함이 본질에 집중하게 해줍니다.

실무에서 직접 DI 컨테이너를 만들 일이 있냐는 질문도 있습니다. 없습니다. 하지만 내부 동작 원리를 이해하면 문제 발생 시 빠르게 해결할 수 있고, 더 나은 설계가 가능하며, 면접에서 깊이 있는 답변을 할 수 있습니다.

Spring만 배우면 안 되냐는 질문에는, 가능하지만 "왜"에 답할 수 없다고 답합니다. "@Component를 왜 쓰나요?"라는 질문에 "그냥 쓰라고 해서요"와 "컴포넌트 스캔을 통해 자동으로 Bean으로 등록되어, 수동 등록의 번거로움을 줄이고 휴먼 에러를 방지할 수 있습니다"는 완전히 다른 답변입니다.

## 핵심 메시지

Spring은 마법이 아닙니다. @Autowired, @Component는 마법처럼 보이지만, 내부적으로는 우리가 Level 3에서 구현한 것과 동일한 원리로 동작합니다.

차이는 Spring의 수년간의 최적화, 수많은 엣지 케이스 처리, 방대한 생태계에 있습니다. 하지만 본질은 같습니다.

이 예제를 통해 Spring의 가치를 이해하고, 내부 동작 원리를 파악하며, 더 나은 설계 능력을 갖추게 되기를 바랍니다.

---

*작성일: 2026-01-30*
