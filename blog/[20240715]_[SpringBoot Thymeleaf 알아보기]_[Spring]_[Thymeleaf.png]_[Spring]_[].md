# SpringBoot와 Thymeleaf 사용법

- 의존성 부여

```
implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
```

- 프로젝트 설정(`application.yaml`)

```yaml
server:
  port: 8082
    
spring:
  application:
    name: est-service-Dev
    
  devtools:
    restart:
      enabled: true # 자동 재시작(로직이 변경 될때마다 파일을 다시 시작할 필요가 없음)
    
  thymeleaf:
    cache: false # 캐시 사용x(수정 시 바로 반영)
    prefix: classpath:/templates/ # 템플릿 파일 위치
    suffix: .html # 템플릿 파일 확장자
    mode: HTML # HTML5와 호환
    encoding: UTF-8 # 인토딩 설정
```

- HTML 작성(index.html)

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>스프링부트 타임리프 홈페이지 테스트</h1>
</body>
</html>
 ```

  - controller 작성

```java
package com.example.demo;
    
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;
    
@Controller
public class ThymeleafController {
    @GetMapping("/index")
    public String index(Model model) {
    return "index";
    }
}
```

- 결과![#](blog/TIL/7:15/indexHTML.png)

## 문법

### 변수 표현식

`th:...${...}` 와 같은 형식으로 모델에서 전달된 데이터를 HTML에 삽입 합니다.

- `<p th:text="${name}">메세지가 표시됩니다.</p>` 이라는 HTML 코드가 있을 때 Controller 객체의 메서드에는 다음과 같이 작성하여 데이터를 추가합니다.

```java
@Controller
public class ThymeleafController(){
   	@GetMapping("/index/2") // URL 주소
	public String index(Model model) {
        model.addAttribute("name", "thymeleaf");
        // 추가하는 데이터
        return "index2";
    }
}
```

- 결과![#](blog/TIL/7:15/index2.png)


`<div th:object="${user}">` 에서 user는 모델에서 전달된 키의 값으로 대체 되고 `model.addAttribute("키", "값");` 형태로 작성 됩니다.

### 선택 변수 표현식

**`{…}`** 을 사용하여 선택된 객체의 속성에 접근할 수 있습니다.

- User 클래스

```java
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private String id;
    private String username;
    private String email;
}
```

- ThymeleafController 클래스

```java
@Controller
public class ThymeleafController {
	@GetMapping("/index")
    public String index(Model model) {
        User max = new User("1", "max", "max@gmail.com");
        model.addAttribute("user", max);
        model.addAttribute("iterData", iterData);
        return "index";
    }
}
```

- index.html

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>스프링부트 타임리프 홈페이지 테스트</h1>
<div th:object="${user}">
    <p>아이디: <span th:text="*{id}"></span></p>
    <p>이름: <span th:text="*{username}"></span></p>
    <p>이메일: <span th:text="${user.email}"></span></p>
</div>
</body>
</html>
```

  부모 태그에 **`th:object="${키}"`** 를 작성 해주고 출력 태그에 **`“*{필드}”`** 만 작성할 수 있지만 코드가 복잡해지면 가독성이 떨어질 수 있기 때문에 출력 태그에도 **`“${키.필드}”`** 형식으로 작성하는게 좋습니다.

- 결과
  ![#](blog/TIL/7:15/indexth:.png)

### 조건문

if문과 unless 를 사용하여 참 또는 거짓이면 해당하는 문구를 출력 합니다.

- ThymeleafController

```java
@Controller
public class ThymeleafController {
    @GetMapping("/index")
    public String index(Model model) {
        User max = new User("1", "max", "max@gmail.com", false, true,"max");
        model.addAttribute("user", max);
        model.addAttribute("iterData", iterData);
        return "index";
    }
}
```

- index.html

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>스프링부트 타임리프 홈페이지 테스트</h1>
<div th:object="${user}">
    <p>아이디: <span th:text="*{id}"></span></p>
    <p>이름: <span th:text="*{username}"></span></p>
    <p>이메일: <span th:text="${user.email}"></span></p>
    <p th:unless="*{admin}">관리자 권한이 있습니다.</p> <!-- 거짓이면 출력 -->
    <p th:if="${user.person}">max는 유저 권한이 있습니다.</p> <!-- 참이면 출력 -->
    <p th:unless="${user.person}">유저 권한이 있습니다.</p> <!-- 거짓이면 출력하지만 person의 값은 참이기 때문에 출력 안됨 -->
</div>
</body>
</html>
```

- 결과![#](blog/TIL/7:15/admin.png)


### 반복문

`th:each` 속성을 사용하여 반복적인 작업을 처리할 수 있습니다.

- ThymeleafController

```java
@Controller
public class ThymeleafController {     
    @GetMapping("/index")
    public String index(Model model) {
        User max = new User("1", "max", "max@gmail.com", false, true,"max");
        int[] iterData = {1, 2, 3, 4, 5};
        model.addAttribute("user", max);
        model.addAttribute("iterData", iterData);
        return "index";
    }
}
```

- index.html

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>스프링부트 타임리프 홈페이지 테스트</h1>
<div th:object="${user}">
    <p>아이디: <span th:text="*{id}"></span></p>
    <p>이름: <span th:text="*{username}"></span></p>
    <p>이메일: <span th:text="${user.email}"></span></p>
    <p th:unless="*{admin}">관리자 권한이 있습니다.</p> <!-- 거짓이면 출력 -->
    <p th:if="${user.person}">max는 유저 권한이 있습니다.</p> <!-- 참이면 출력 -->
    <p th:unless="${user.person}">유저 권한이 있습니다.</p> <!-- 거짓이면 출력하지만 person의 값은 참이기 때문에 출력 안됨 -->
</div>
<ul th:each="item:${iterData}">
    <li th:text="${item}"></li>
</ul>
</body>
</html>
```

- 결과![#](blog/TIL/7:15/foreach.png)


### 폼 바인딩

`th:object`, `th:field` 속성을 사용하여 폼과 객체를 바인딩할 수 있습니다.

- index.html

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>스프링부트 타임리프 홈페이지 테스트</h1>
<div th:object="${user}">
    <p>아이디: <span th:text="*{id}"></span></p>
    <p>이름: <span th:text="*{username}"></span></p>
    <p>이메일: <span th:text="*{email}"></span></p>
    <p th:unless="*{admin}">관리자 권한이 있습니다.</p> <!-- 거짓이면 출력 -->
    <p th:if="${user.person}">max는 유저 권한이 있습니다.</p> <!-- 참이면 출력 -->
    <p th:unless="${user.person}">유저 권한이 있습니다.</p> <!-- 거짓이면 출력 -->
</div>
<ul th:each="item:${iterData}">
    <li th:text="${item}"></li>
</ul>
<a th:href="@{/index/2}">인덱스2 페이지로 이동</a><br>
<a th:href="@{/index/3}">인덱스3 페이지로 이동</a>

<form th:action="@{/users}" method="post">
    <input type="text" name="username">
    <input type="email" name="email">
    <button type="submit">저장</button>
</form>
</body>
</html>
```

- ThymeleafController

```java
@Controller
public class ThymeleafController {

    @GetMapping("/index")
    public String index(Model model) {
        User max = new User("1", "max", "max@gmail.com", false, true,"max");
        int[] iterData = {1, 2, 3, 4, 5};
        model.addAttribute("user", max);
        model.addAttribute("iterData", iterData);
        return "index";
    }

    @GetMapping("/index/2")
    public String index2(Model model) {
        model.addAttribute("name", "thymeleaf");
        // 추가하는 데이터
        return "index2";
    }

    @GetMapping("/index/3")
    public String index3(Model model) {
        return "index3";
    }

    @PostMapping("/users")
    public String users(@ModelAttribute User user) {
        System.out.println("유저의 이름은 " + user.getUsername() + "입니다.");
        System.out.println("유저의 이메일은 " + user.getEmail() + "입니다.");
        return "index2";
    }
}
    
```

- 결과(터미널에 결과가 나옴)![#](blog/TIL/7:15/form.png)
