# DI(Dependency Injection)

## DI란?

* 객체간의 의존성을 외부에서 주입하는 디자인 패턴
* 객체가 직접 다른 객체를 생성 하거나 의존성을 관리하는 대신 외부에서 의존성을 제공받는 방식
* 예시1 (DI를 적용하지 않은 코드)
   ```java
   public class Car{
       private Engine engine;
       
       public Car(){
           this.engine = new Engine();
       }
   }
   public class Engine{
       
   }
   ```
  Car 클래스는 Engine 객체를 직접 생성하고 있는데 이러한 경우 클래스간의 결합도가 높아서 코드의 유연성이 떨어지고
  객체의 변화가 다른 객체에 직접적인 영향을 줍니다.
* 예시2 (DI를 적용한 코드)
   ```java
  public class Car{
       private Engine engine;
       
       public Car(Engine engine){
            this.engine = engine;
       }
   }
   public class Engine{
       
   }
  ```
  Car 클래스에서 Engine 객체를 직접 생성하지 않고 생성자의 매개변수를 통해 외부에서 주입 받게 되면서
  클래스 간의 결합도를 낮추고 코드의 유연성을 높일 수 있습니다.

## Spring에서 사용되는 방법

* 주로 `@Service`, `@Component`, `@Autowired`등 과 같은 어노테이션을 사용하여 의존성을 주입 합니다.

| 어노테이션 표기(@~~) |                                                 설명                                                 |
|:-------------:|:--------------------------------------------------------------------------------------------------:|
|  @Autowired   |                  IoC 컨테이너가 필요한 빈을 자동으로 주입 해숩니다. <br/>필드, 생성자, 세터메서드에서 사용할 수 있습니다.                  |
|    @Inject    |                                           @AutoWired와 동일                                           |
|  @Qualifier   |         특정 개체에 의존성을 주입하는데 사용합니다.<br/> 필드, 생성자, 세터메서드에서 사용가능하고 생성자에 사용할 경우 매개변수 앞에 작성해 줍니다.         |
|   @Resource   | 빈의 이름을 기반으로 의존성을 주입해주는 어노테이션 입니다.<br/> @AutoWired와 달리 빈의 이름을 지정해서 의존성을 주입할 수 있습니다.(@Qualifier과 비슷) |

## 같은 인터페이스를 구현한 Bean 객체가 여러개일 경우

* 같은 인터페이스를 구현한 Bean 객체가 여러개일때는 <b>`@Primary`</b>나  <b>`@Qualifier`</b>와 같은 구분자를 사용하지
  않는다면 `NoUniqueBeanDefinitionException`예외가 발생
* 해결방안
    * `@Primary`, `@Qualifier`사용
    * 생성자 주입시 특정 구현체 명시 `생성자(@Qualifier("") 매개변수){}`과 같은 형식으로 작성
    * `@Autowired` 사용시 구현체 명시

# PSA(Portable Service Abstraction) [이동식 서비스 추상화]

## PSA란?

* pring Framework에서 제공하는 추상화(시스템의 공통 기능을 뽑아서 분리 시키는것) 개념

### 장점

1. 유연성: 서비스 객체의 코드를 변경하지 않고 내부의 비즈니스 로직만 바꿔주면 변경 가능
2. 유지보수성: 서비스 객체의 코드를 변경하지 않기 때문에 유지보수시 변경할 범위가 특정 됨
3. 테스트 용이성: 서비스 객체와 기술 구현을 분리하여 단위 테스트에 편리합니다.

### 단점

1. 복잡성 증가: 추상화 계층이 추가되기 때문에 코드가 복잡해 질 수 있습니다.
2. 성능 오버헤드: 컴퓨터의 자원을 많이 잡아먹습니

## PSA 적용X 코드

```java
public class EmailService {
    public void sendEmail(String to, String subject, String body) {
        // SMTP를 사용하여 이메일 발송
        // ...
    }
}
```

## PSA 적용 코드

```java
// interface 생성
public interface EmailSender {
    void sendEmail(String to, String subject, String body);
}

// interface 구현체1
public class AWSEmailService implements EmailSender {

    @Override
    public void sendEmail(String to, String subject, String body) {
        // AWS 기술을 활용한 로직
        System.out.println(to + "에게 AWS로 이메일 보내기");
    }
}

// interface 구현체2
public class SMTPEmailService implements EmailSender {

    @Override
    public void sendEmail(String to, String subject, String body) {
        // SMTP 기술을 활용한 로직
        System.out.println(to + "에게 SMTP로 이메일 보내기");
    }
}

// EmailService 코드
public class EmailService {
    private final EmailSender emailSender;

    public EmailService(EmailSender emailSender) {
        this.emailSender = emailSender;
    }

    public void sendEmail(String to, String subject, String body) {
        emailSender.sendEmail(to, subject, body);
    }
}

// 위 코드를 활용하여 출력
public static void main(String[] args) {
    EmailService emailService = new EmailService(new AWSEmailService());
    emailService.sendEmail("나", "asd", "하이");
    EmailService emailService1 = new EmailService(new SMTPEmailService());
    emailService1.sendEmail("나", "asd", "하이");
}
```

> 💡 자바에서 배웠던 interface와 비슷함

# AOP(Aspect-Oriented Programming): 관점지향 프로그래밍

## AOP란?

<img src="blog/TIL/7:01/img.png" alt="관점지향 프로그래밍(AOP) 이란?">

## AOP의 개념

1. <b>Aspect</b>: 공통된 로직을 모듈화 한 것(주로 로깅, 트랜젝션 관리, 보안과 같은 여러 클래스가 동일하게 사용하는 로직을 모듈화 합니다.)
2. <b>Join Point</b>: Aspect가 적용될 수 있는 지점(주로 메서드 호출, 필드 접근과 같은 위치에 적용)
3. <b>Pointcut</b>:  어떤 조인 포인트에 애스펙트가 적용될지를 결정
4. <b>Advice</b>: 특정 조인 포인트에서 실행되는 코드(공통된 로직을 제외한 부가적인 로직)
5. <b>Advice</b>: 애스펙트를 실제 객체에 적용하여 어드바이스가 적절한 지점에서 실행되도록 하는 과정

> 💡 <b>관점 지향 프로그래밍은 여러 객체에서 동일한 작업을 반복해야하는 로직이 있을 때 사용</b>

# MVC(Model-View-Controller)패턴

## MVC 패턴이란?

* 웹 애플리케이션 개발에서 사용되는 아키택쳐 패턴입니다.
* Model, View, Controller 세 가지 주요 컴포넌트로 나타냅니다.(M, V, C를 각 계층이라고 합니다.)

```java
// Model 객체
@Getter
@Setter
@AllArgsConstructor
public class User {
    private String name;
    private String name;
    private int age;
}

public class UserView {

}

// View 객체
public class UserView {
    public void printUserDetails(User user) {
        System.out.println("User details");
        System.out.println("Name: " + user.getName());
        System.out.println("Email: " + user.getEmail());
        System.out.println("Age: " + user.getAge());
    }
}

// Controller 객체
@AllArgsConstructor
public class UserController {
    private User model;
    private UserView view;

    public void updateView() {
        view.printUserDetails(model);
    }

    public void setUserName(String name) {
        model.setName(name);
    }

    public void setUserAge(int age) {
        model.setAge(age);
    }
}
```

* 결과<br><img src="blog/TIL/7:01/MVC.png" alt="#">

1. **Model**
    * 사용자의 정보를 저장하는 객체
    * 비즈니스 로직과 데이터 표현을 담당
    * 데이터의 상태를 관리하고 조작하
2. **view**
    * 사용자에게 정보를 표시하는 역할
    * Model의 데이터를 사용하여 사용자 인터페이스를 나타냄
    * 사용자와의 상호작용을 처리
3. **controller**
    * Model과 View 사이의 상호작용을 제어
    * 사용자의 입력을 받아 Model을 업데이트하고, 업데이트된 Model을 View에 전달합니다.
    * 사용자의 요청을 해석하고 적절한 동작을 수행

## 퀴즈

요구사항:

1. Model 클래스 (Quiz.java)
    - String 타입의 question과 answer 필드를 가집니다.
    - 생성자를 통해 question과 answer를 초기화합니다.
    - getQuestion()과 getAnswer() 메서드를 제공합니다.
2. View 클래스 (QuizView.java)
    - displayQuestion(String question) 메서드: 주어진 question을 출력합니다.
    - getUserAnswer() 메서드: 사용자로부터 답변을 입력받아 반환합니다.
    - displayResult(boolean isCorrect) 메서드: 퀴즈의 결과를 출력합니다. isCorrect가 true이면 "정답입니다!"를, false이면 "오답입니다."를 출력합니다.
3. Controller 클래스 (QuizController.java)
    - Quiz 객체와 QuizView 객체를 필드로 가집니다.
    - 생성자를 통해 Quiz 객체와 QuizView 객체를 초기화합니다.
    - startQuiz() 메서드: 퀴즈를 시작하고 진행합니다.
        - QuizView의 displayQuestion() 메서드를 호출하여 질문을 출력합니다.
        - QuizView의 getUserAnswer() 메서드를 호출하여 사용자의 답변을 받습니다.
        - 사용자의 답변과 Quiz 객체의 정답을 비교하여 결과를 판단합니다.
        - QuizView의 displayResult() 메서드를 호출하여 결과를 출력합니다.
4. 메인 클래스 (Main.java)
    - Quiz 객체를 생성하고 질문과 정답을 초기화합니다.
    - QuizView 객체를 생성합니다.
    - QuizController 객체를 생성하고 Quiz 객체와 QuizView 객체를 전달합니다.
    - QuizController의 startQuiz() 메서드를 호출하여 퀴즈를 시작합니다.

<details>
  <summary>자바 코드 보기</summary>

```java
// Quiz Model
@Getter
@Setter
@AllArgsConstructor
public class Quiz {
    private String question;
    private String answer;
}

// Quiz View
public class QuizView {
    public void displayQuestion(String question) {
        System.out.println("문제: " + question);
    }

    public String getUserAnswer() {
        Scanner sc = new Scanner(System.in);
        System.out.print("정답: ");
        String answer = sc.nextLine();
        sc.close();
        return answer;
    }

    public void displayResult(Boolean isCorrect) {
        if (isCorrect) {
            System.out.println("정답입니다");
        } else {
            System.out.println("오답입니다");
        }
    }
}

// Quiz Controller
private Quiz quiz;
private QuizView quizView;

public QuizController(QuizView quizView, Quiz quiz) {
    this.quiz = quiz;
    this.quizView = quizView;
}


public void startQuiz() {
    quizView.displayQuestion(quiz.getQuestion());
    String answer = quizView.getUserAnswer();

    boolean isCorrect = checkAnswer(answer);

    quizView.displayResult(isCorrect);
}

private boolean checkAnswer(String answer) {
    return answer.equalsIgnoreCase(quiz.getAnswer());
}
```

</details>

# SpringBoot Layer(계층)

1. 프레젠테이션 계층 (Presentation Layer)
    * 사용자 인터페이스와 직접 상호작용 하는 계층으로 사용자의 요청을 받아서 처리하고 결과를 반환
    * 사용자 입력의 유효성을 검사하고 적절한 응답을 생성
    * 주로 웹 페이지, REST API, 그리고 컨트롤러가 여기에 속합니다
    * **`Controller`**: Spring MVC의 @Controller 또는 @RestController 어노테이션을 사용하여 웹 요청을 처리
    * **`View`**: 템플릿 엔진을 사용해서 화면을 생성
2. 서비스 계층 (Service Layer)
    * 비즈니스 로직을 구현하고, 프레젠테이션 계층와 데이터 접근 계층 사이의 중간 계층 역할
    * 트랜잭션 관리, 보안, 비즈니스 규칙 등을 처리
    * 도메인 모델과 밀접한 연관
    * **`@Service`** 어노테이션을 이용하여 비즈니스 로직 구현
3. 데이터 접근 계층 (Data Access Layer)
    * 데이터베이스와의 상호작용을 담당
    * **`@Repository`** 어노테이션을 사용하여 정의
    * CRUD(Create, Read, Update, Delete)작업을 처리(JPA를 이용하면 DB 작업을 추상화 가능)

## 계층간의 상호작용

* Presentation Layer는 Service Layer를 호출해서 비즈니스 로직 실행
* Service Layer는 Data Access Layer를 이용하여 CRUD 작업 수행
* 계층간의 의존성은 인터페이스를 이용하여 약결합 상태를 유지

## 장점

* 가독성 및 유지보수성 향상(코드를 살짝 봤는데 뭐가 뭔지 잘 모르겠음)
* 계층간의 의존성이 낮아서 단위 테스트하기 좋음
* 새로운 기능을 확장및 추가를 할때 다른 계층에 미치는 영향이 적음

# 회고

* PSA 부분은 이전에 자바를 할 때 interface 에서 배웠던 내용들에서
  크게 달라진 부분이 없는 느낌 이었어서 나름 따라갈만했음
* DI(의존성 주입)는 객체 내부에서 직접 다른 객체를 생성해서 관리 하는것이 아닌 생성자 매개변수를 통해 외부에서 의존성을 제공받는 것 정도로 이해함
* AOP는 "공통된 로직을 따로 모듈화 시켜서 각 객체에서 그 외에 공통되지 않은 로직에 적용 시켜서 실행 시킨다"
  정도로만 이해함
* 계층은 솔직히 수업할 때 좀 못따라갔어서 녹화 영상 보고 복습이 필요함

> 코딩 테스트도 준비를 해야하는데 당장은 스프링 따라 가는데 시간을 많이 투자를 해야할거 같음...
