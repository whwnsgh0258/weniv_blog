# IoC 컨테이너(매우 중요)

## IoC 컨테이너란?

* IoC는 **Inversion of Control(제어의 역전)**의 약자입니다.
* 객체를 생성하고 관리합니다.(new로 객체 생성 안해도됨)
* 개발자가 IoC 컨테이너에 필요한 객체를 요청하면 해당 객체를 만들어 줍니다.

## 장점

* **낮은 결합도**:
* **코드 재사용성 증가**: 객체의 생성돠 관리를 컨테이너에 맡기면, 해당 객체를 여러 곳에서 사용할 수 있습니다.
* **생산성 향상**: 객체 관리를 직접 하지않고 컨테이너에 맡겨서, 개발자는 비즈니스 로직 구현에 좀 더 신경 쓸 수 있습니다.

## 단점

## 결합도란?

모듈 또는 컴포넌트 간의 상호 의존성의 정도를 나타냅니다.

### 강결합(Tight Coupling)

* 모듈간의 의존성이 높아서 하나의 모듈이 변경되어도 다른 모듈에 큰 영향을 미치는 상태 입니다.

```java
public class UserService {
    private UserRepository userRepository;

    public UserService() {
        this.userRepository = new UserRepository();
    }

    public void createUser(User user) {
        userRepository.save(user);
    }
}

public class UserRepository {
    public void save(User user) {
        // 사용자 저장 로직
    }
}
```

### 약결합(Loose Coupling)

* 모듈간의 의존성이 낮아서 모듈이 다른 모듈에 미치는 영향이 작은 상태 입니다.

```java
// 자바코드로 의존성 낮추기
public interface UserRepositoryInterface {
    void save(User user);
}

public class UserRepository implements UserRepositoryInterface {
    @Override
    public void save(User user) {
        // 사용자 저장 로직
    }
}

public class UserService {
    private UserRepositoryInterface userRepository;

    public UserService(UserRepositoryInterface userRepository) {
        this.userRepository = userRepository;
    }

    public void createUser(User user) {
        userRepository.save(user);
    }
}
```

클래스가 아닌 인터페이스에 의존 함으로써 객체간의 의존성을 낮추고 클래스가 다른 클래스에 미치는 영향을 최소화 할 수 있습니다.

**SOLID**에서 **DIP**원칙을 따라서 **추상화(interface)**에 의존한다고 생각하면 될듯

### 약결합의 장점

1. **유지보수성** 향상: 모듈간의 의존성이 낮아서 변경을 했을 때 영향을 주는 범위가 제한되기 때문에 코드의 수정에 좋음
2. **확장성**: 유지보수성과 같은 이유로 새로운 기능을 추가할때 다른 객체에 미치는 영향이 적어서 좋음
3. **테스트**: 모듈간의 의존성이 낮아서 독립적인 테스트가 가능하고, 단위 테스트 작성이 수월함


> 💡 약결합 코드를 작성하기 위해서 **`interface`**, **`의존성 주입`**, **`IoC 컨테이너`** 등의 기술을 활용하여 유연하고 확장 가능한 코드를 작성할 수 있습니다.


---

## 일반코드 VS Spring IoC 컨테이너

* 일반 Java 코드에서는 객체를 직접 생성하고 관리해야 합니다.

  → **`new`** 키워드로 관리해줌

* IoC 컨테이너를 사용하면 객체의 생명주기를 컨테이너가 관리해 줍니다.

### 일반코드

```java
public class UserService{
	private UserRepository userRepository;
	
	public UserService(){
		this.userRepository = new UserRepository();
		// new로 객체 생성해줌
	}
}
```

### Spring IoC 컨테이너

```java
@Service
public class UserService{
	private UserRepository userRepository;
	
	public UserService(UserRepository userRepository){
		this.userRepository = userRepository;
		// new 없이 객체 생성
	}
}

@Repository
public class UserRepository {
    // ...
}
```

**`UserRepository`**를 생성자를 통해 주입받아서 **`UserService`**와 **`UserRepository`**간의 결합도를 낮춰줍니다.

# Spring Bean

* Spring IoC 컨테이너에 의해 생성되고 관리되는 객체

## Java 객체와 Spring Bean의 차이점

### Java 객체

1. **생성 및 관리**: 개발자가 명시적으로 생성하고 관리합니다.
2. **의존성 관리**: 의존성을 직접 설정하고 주입해야 합니다.
3. **생명주기**: 객체의 생명 주기를 개발자가 제어합니다.

### Spring Bean

1. **생성 및 관리**: Spring IoC 컨테이너가 생성하고 관리합니다.
2. **의존성 관리**: 컨테이너가 자동으로 의존성을 주입합니다 (DI, Dependency, Injection).
3. **생명주기**: 컨테이너가 생명 주기를 관리하며, 초기화 및 소멸 메서드를 지원합니다.

## Spring Bean의 특징

1. 객체 생성과 관리: Spring IoC 컨테이너가 Bean의 생성, 의존성 주입, 소멸 등의 생명주기를 관리 합니다.
2. 싱글톤 패턴: 기본적으로 Bean은 Singleton 패턴으로 생성되어 어플리케이션 전체에서 하나의 인스턴스만 존재합니다.
3. 의존성 주입: Bean은 생성자, Setter 메서드, field 주입 등을 통해 다른 Bean을 주입받습니다.
   (`@Component`, `@Service`, `@Repository` 등의 어노테이션을 통해 Bean을 정의합니다)

>💡 위 방법들 외에도 **XML 설정, 자바 클래스 설정(@Configuration, @Bean), 프로퍼티 파일 설정(@value, Environment)** 등으로 의존성을 주입할 수 있습니다.

# 싱글톤 패턴 구현 방법

## 싱글톤 패턴이란?

- 클래스의 인스턴스를 단 하나만 생성하고 어디서든 해당 인스턴스에 접근할 수 있도록 하는 디자인 입니다.
- Spring 에서 Bean들은 특별한 특징을 주지 않는 이상 기본적으로 Singleton 객체로 동작합니다.
- 인스턴스가 하나이기 때문에 멀티 스레드 환경에서 동시성 이슈가 생길 수 있음.

```java
// 싱글톤 인스턴스 생성
import lombok.Getter;

public class Singleton {
    @Getter
    private static final Singleton instance = new Singleton();

    private Singleton() {
    }
}
// 출력
@SpringBootApplication
public class BasicApplication {

    public static void main(String[] args) {

        Singleton singleton = Singleton.getInstance();
        Singleton singleton1 = Singleton.getInstance();
        Singleton singleton2 = Singleton.getInstance();

        System.out.println(singleton);
        System.out.println("====================================");
        System.out.println(singleton1);
        System.out.println("====================================");
        System.out.println(singleton2);

        //SpringApplication.run(BasicApplication.class, args);
    }
}
```