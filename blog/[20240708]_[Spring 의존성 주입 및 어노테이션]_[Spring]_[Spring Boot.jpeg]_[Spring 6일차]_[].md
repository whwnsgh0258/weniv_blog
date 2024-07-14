
# @SpringBootApplication 어노테이션

<img src="blog/TIL/7:08/SpringBootApplication.png" alt="#">

## @SpringBootApplication이란?

**`@SpringBootApplication`** 은 Spring Boot 어플리케이션의 시작점을 나타내는 어노테이션으로 3가지 어노테이션의 기능을 결합하여 제공합니다.

### 기능

1. `@Configuration`: 클래스를 Spring 프로젝트의 설정 클래스로 지정
2. `@EnableAutoConfiguration`: 스프링 부트가 자동으로 설정 해주는 빈을 활성화 합니다.
3. `@ComponentScan`: 지정된 패키지와 그 하위의 패키지를 스캔하는 작업을 합니다. <br> (`@ComponentScan`을 명시적으로 지정 해주지 않으면 `@SpringBootApplication`
   이나 `@Configuration`이 지정된 클래스 패키지와 그 하위 패키지를 스캔 합니다.)

<details>
   <summary><B>@SpringBootApplication 동작 과정 과 예시</B></summary>
<li>
   동작과정
</li>

``` text
+--------------------------+
|  @SpringBootApplication  | 여기서 어플리케이션 시작
+--------------------------+
             |
             |
             V
+--------------------------+
|      @Configuration      | @SpringBootApplication이 있는 클래스를 설정 클래스로 지정
+--------------------------+
             |
             |
             V
+--------------------------+
| @EnableAutoConfiguration | 스프링 부트가 자동으로 설정 해주는 빈을 활성화
+--------------------------+
             |
             |
             V
+--------------------------+
|      @ComponentScan      | 지정된 패키지와 그 하위 패키지에서 스캔
+--------------------------+
             |
             |
             V
+--------------------------+
|     Spring Container     |
+--------------------------+

```

<li>
예시
</li>

```java

package com.example;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.stereotype.Service;
import org.springframework.stereotype.Repository;

@SpringBootApplication(scanBasePackages = "com.example") // 해당 패키지를 포함한 하위 패키지를 모두 스캔
public class MyApp {
    public static void main(String[] args) {
        SpringApplication.run(MyApp.class, args);
    }
}

// 컨트롤러 패키지 (MVC 패턴에서 Controller에 해당)
@RestController
public class MyController {
    @GetMapping("/hello") // API 앤드포인트
    public String hello() {
        return "하이"; // Spring Boot를 실행하고 localhost:8080/hello 로 접속하면 "하이"를 반환
    }
}

// 서비스 패키지 (MVC 패턴에서 Service에 해당)
@Service
public class MyService {
    // 서비스 로직 (비즈니스 로직)
}

// 레포지토리 패키지 (MVC 패턴에서 Repository에 해당)
@Repository
public interface MyRepository {
    // 레포지토리 로직
}
```

<li>
<b>@ComponentScab 어노테이션 주의 사항</b>: <code>@SpringBootApplication</code> 어노테이션에 붙어있는 클래스의 위치를 기준으로 해당 패키지와 그
하위 패키지에서 어노테이션이 붙은 클래스를 탐색하여 빈을 탐색하기 때문에 다른 패키지를 스캔하기 위해서는 별도의 설정이 필요합니다.
</li><br>

<li>예시</li>

```text
com.example
├── main 
│   └── Application.java ◄─── @SpringBootApplication
├── service
│   └── UserService.java
└── util
    └── Helper.java
위 구조로 프로젝트를 실행 시키면 main 패키지의 Application.java 만 스캔하고 service 패키지와 utill 패키지는 스캔하지 않습니다.
```

```text
com.example
├── main 
│   └── Application.java ◄─── @SpringBootApplication(scanBasePackages = "com.example.main")
├── service
│   └── UserService.java
└── util
    └── Helper.java
이런 식으로 Application.java 클래스의 어노테이션에서 스캔의 범위를 조정하면 service, util 패키지를 스캔할 수 있습니다.
```

</details>

# 스프링 의존성 주입 방법(DI)

## 1. Setter 메서드로 의존성 주입

- 특징 및 방법<br>
  세터 메서드에 `@Autowired` 어노테이션을 붙혀 의존성을 주입 받습니다.

```java

@Service
public class MyService {
    private MyDependency myDependency;

    @Autowired // 세터 메서드에 의존성 주입
    public void setMyDependency(MyDependency myDependency) {
        this.myDependency = myDependency;
    }
}
```

### 장단점

#### 장점

- 선택적으로 의존성을 주입할 때 사용할 수 있습니다.
- 의존성을 나중에 변경할 수 있습니다.

#### 단점

- 의존성 주입이 객체 생성 이후에 이루어지기 때문에 의존성이 주입되지 않은 상태로 객체가 사용될 수 있습니다.
- 세터 메서드를 public으로 노출해야 하기 때문에 캡슐화가 약화될 수 있습니다.
- 외부에서 데이터를 조작할 가능성이 존재하기 때문에 이후에 심각한 에러가 발생할 수 있습니다.

## 2. 필드 의존성 주입

- 특징 및 방법<br>
  클래스의 필드에 `@Autowired` 어노테이션을 붙혀 의존성을 주입 합니다.

```java

@Service
public class MyService {
    @Autowired // 필드에 의존성 주입
    private MyDependency myDependency;
}
```

### 장단점

#### 장점

- 코드가 간결해 집니다.
- 의존성 주입을 위한 별도의 코드가 필요하지 않습니다.

#### 단점

- 의존성이 명시적으로 표현되지 않아서 코드의 가독성이 떨어질 수 있습니다.
- 필드에 상수(final)가 선언되는것이 아니기 때문에 불변성을 확보하기 어렵습니다.
- 의존성을 변경하기 어렵습니다.

## 3. 생성자 메서드 의존성 주입

- 특징 및 방법<br>
  클래스의 생성자를 통해 의존성을 주입하는 방법으로 생성자의 매개변수로 의존성을 받아들이고 주입받은 의존성을 클래스 내부에서 사용합니다.
  (@Autowired 어노테이션이 필요없음)

```java

@Service
@RequiredArgsConstructor // 생성자를 만들어줘야 하는 필드에 한해서 생성자 메서드 생성
public class MyService {
    private final MyDependency myDependency;
}
```

### 장단점

#### 장점
- 의존성이 명확하게 표현되어 코드의 가독성이 좋습니다.
- 의존성이 객체 생성시에 주입 되므로 객체가 생성될 때 모든 의존성이 설정 된것을 보장 합니다.
- final을 사용하여 불변성을 높히고 생성자 주입을 사용하면 의존성 주입 후 변경이 불가 합니다.

#### 단점
- 의존성이 많아지면 생성자의 매개변수가 많아지기 때문에 코드의 복잡성이 높아집니다.

## 권장되는 의존성 주입 방식

주로 생성자 주입 방식이 권장 됩니다.

### 이유
1. **불변성 보장**:<br>
   의존성이 객체 생성 시에 주입되므로, 주입된 의존성은 final로 선언될 수 있습니다. 이는 객체의 불변성을 보장합니다.
2. **한 타입 체크**:<br>
컴파일 시점에 의존성의 주입이 보장됩니다. 생성자를 통해 주입되므로, 객체가 생성될 때 모든 필요한 의존성이 제공되지 않으면 컴파일 오류가 발생합니다.
3. **테스트 용이성**:<br>
   생성자 주입은 객체 생성 시 필요한 모든 의존성을 명시적으로 요구하기 때문에, 단위 테스트 작성 시 모킹(mocking)과 같은 작업이 수월해집니다.
4. **순환 의존성 문제 방지**:<br>
   생성자 주입은 순환 의존성을 조기에 감지할 수 있어, 설계 단계에서 문제를 발견하고 수정할 수 있습니다.

# 회고
1. 의존성 주입 방식과 왜 생성자를 통해 의존성을 주입하는 것이 권장되는지 이해했음(근데 완전 이해해서 누가 물어보면 딱 자세하게 말할 수 있는 정도는 아니라서 반복해서 보면서 암기를 해야함)
2. 스프링 부트 계층에 대해서는 아직 좀 햇갈리는 부분이 있음(이해가 안되는건 아닌데 교안을 안보고 생각해보면 어떤 계층에서 어떤 로직을 작성하는지 바로 생각이 잘 안남... 조금 더 봐야할듯)
3. Spring에서 요청을 처리하는 흐름은 조금 보충이 필요할 것 같음 
4. 개발과는 조금 상관없는 이야기 일수도 있지만 요즘 공부를 하면서 느끼는건데 공부를 잘하는 사람은 아는게 많은 사람이 아니라 자기가 모르는게 뭔지 아는사람 인거같음.<br>
   (질문에서 그사람의 수준을 평가할 수 있다는게 이런 의미인가 싶음)

## 이번주 목표
1. 코테 하루 한문제 이상은 풀어보기(일단 간단한거 부터)
2. 수업 시간에 배운 내용을 바탕으로 혼자 실습 해보면서 이해하기
