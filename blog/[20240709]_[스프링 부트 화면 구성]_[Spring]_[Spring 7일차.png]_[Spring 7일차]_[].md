# Spring 화면 구성

## 템플릿 엔진

웹 어플리케이션에서 동적인 웹 페이지를 생성하기 위해 사용되는 도구로서 미리 정의된 템플릿에 데이터를 삽입하여 HTML 페이지를 생성 합니다.

### Thymeleaf 템플릿

- **특징**
    - HTML 파일 자체를 템플릿으로 사용하기 때문 HTML 구조를 웹 브라우저에서 확인이 가능 합니다.(정적 파일로 보는게 가능)
    - 다양한 표현식을 지원하기 때문에 조건문, 반복문 등을 사용할 수 있습니다.
    - MVC 패턴에서 쉽게 사용할 수 있습니다.
- **장점**
    - 스프링 부트를 실행 시키지 않아도 정적 HTML 파일을 열어볼 수 있습니다.
    - th 를 사용하여 서버에 렌더링 되었을 때 다양한 속성 추가가 가능 합니다.
- **예시**<br>
  의존성 추가
  ```
  implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
  ```
  html 파일
    ```html
    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org" lang="ko">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
    <h1>스프링 부트 타임리프 홈페이지 테스트</h1>
    <p th:text="${name}">메세지가 표시 됩니다</p>
    <!-- th:text="${name}"는 나중에 controller 에서 설정 해줌-->
    <p th:text="${name2}">메세지가 표시 됩니다</p>
    <p th:text="${name3}">메세지가 표시 됩니다</p>
    <p th:text="${name4}">메세지가 표시 됩니다</p>
    <h2 th:style="${style}" th:text="${text}">헤더2</h2>
    </body>
    </html>
    ```

위 코드를 Spring Boot 서버에 띄우기 위해서는 Controller 클래스에서 HTTP 메서드를 사용하여 불러옵니다.

# HTTP

HTTP(HyperText Transfer Protocol)는 클라이언트와 서버를 연결하기 위한 프로토콜 입니다.

## HTTP 메서드

HTTP 메서드는 클라이언트가 서버에게 요청의 목적이나 종류를 알리는 수단 입니다.

### 1. GET

- 서버에서 리소스(데이터)를 조회할 때 사용합니다.
- 데이터를 쿼리 문자열로 전달 합니다.
- 예시: 나이가 25인 데이터 조회(`GET /user?age=25`)

### 2. POST

- 서버에 새로운 리소스(데이터)를 생성할 때 사용합니다.
- 데이터를 요청 본문(Request body)에 포함하여 전달 합니다.
- 예시: `POST /user`(생성 하고자 하는 데이터는 요청 본문에 포함)

### 3. PUT

- 서버에 존재하는 모든 리소스(데이터)를 갱신 합니다.
- 예시: `PUT /user/1`(갱신 하고자 하는 데이터는 요청 본문에 포함)

### 4. PATCH

- 서버에 존재하는 리소스(데이터) 중 일부를 갱신 합니다.
- 예시: `PATCH /user/1`(수정 하고자 하는 데이터는 요청 본문에 포함)

### 5. DELETE

- 서버에 존재하는 리소스(데이터)를 삭제 합니다.
- 예시: `DELETE /user/1`

## HTTP 상태 코드

서버의 응답 결과를 나타내는 코드

- 1~~: 처리중
- 2~~: 성공
    - 200(OK): 요청이 성공적으로 처리됨
    - 201(Create): 새로운 데이터가 생성됨
- 3~~: 리다이렉
    - 301(Moved Permanently): 요청한 데이터가 영구적으로 이동
    - 302(Found): 요청한 데이터가 일시적으로 이
- 4~~: 클라이언트 오류
    - 400(Bad Request): 잘못된 요청
    - 401(Unauthorized): 인증되지 않은 사용자의 요청
    - 403(Forbidden): 접근 권한이 없음
    - 404(Not Found): 요청한 리소스를 찾을 수 없음
- 5~~: 서버 오류
    - 500(Internal Server Error): 서버 내부 오류
    - 503(Service Unavailable): 서버가 일시적으로 요청을 처리할 수 없음

> **401과 403의 차이점**   
> **401**: 리소스에 접근하는 클라이언트가 누구인지도 모름   
> **403**: 리소스에 접근하는 클라이언트가 인증 되어있어서 누구인지는 아는데 해당 리소스에는 접근할 수 있는 권한이 없음

# 실습

의존성

```
implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
```

view

```html
<!-- HTML 코드 -->
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>스프링 부트 타임리프 홈페이지 테스트</h1>
<p th:text="${name}">메세지가 표시 됩니다</p>
<p th:text="${name2}">메세지가 표시 됩니다</p>
<p th:text="${name3}">메세지가 표시 됩니다</p>
<p th:text="${name4}">메세지가 표시 됩니다</p>
<h2 th:style="${style}" th:text="${text}">헤더2</h2>
</body>
</html>
```

@Controller

```java
// @Controller 코드
package com.example.demo;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class ThymeleafController {

    @GetMapping("/index")
    public String index(Model model) {
        model.addAttribute("name", "굿");
        model.addAttribute("name2", "굿굿");
        model.addAttribute("name3", "굿굿굿");
        model.addAttribute("name4", "굿굿굿굿");
        model.addAttribute("name5", "굿굿굿굿굿");
        model.addAttribute("style", "color: blue");
        model.addAttribute("text", "글자 색은 파랑색");
        return "index";
    }
}

```