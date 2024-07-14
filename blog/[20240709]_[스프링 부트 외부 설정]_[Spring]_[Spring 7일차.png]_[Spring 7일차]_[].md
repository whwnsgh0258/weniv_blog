# 스프링 부트 외부 설정을 통한 변수 사용

## 외부 설정이란?

어플리케이션을 제어하기 위하여 외부에서 주입되는 설정값 입니다.<br>
어플리케케이션, 데이터베이스, 캐싱, API, 서버 등 여러가지 정보들을 코드상에 직접 나타내지 않고 yaml 파일에서 작성해 줍니다.

### 주로 외부 설정 파일에 들어가는 정보(yaml 파일 기준으로 작성)

#### 1. 어플리케이션 설정

어플리케이션의 이름, 버전 등의 기본적인 정보들을 설정 합니다.

```yaml
spring:
  application:
    nane: MyApp # 어플리케이션의 이름
    version: 1.0.0 # 어플리케이션의 버전 정보
```

#### 2. 서버 설정

서버 포트 및 컨택스트 경로를 설정 합니다.

```yaml
server:
  port: 8081 # 스프링의 기본 포트는 8080 입니다.

```

#### 3. 데이터베이스 설정

어플리케이션과 연결할 DB의 URL, 사용자명, 비밀번호, 드라이버 비밀번호등을 설정 합니다.

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/DB table name # 어플리케이션이 연결할 DB의 경로(포트 번호는 수정 가능)
    username: your DB username # DB의 username(기본설정은 root로 되어있음)
    password: your DB password # DB의 비밀번호
    driver-class-name: com.mysql.cj.jdbc.Driver # MySQL 라이브러리가 포함된 클래스의 경로(MySQL이 이난 다른 DB를 사용하면 이거 바꿔주면 됨)
```

#### 4. JPA 설정

Hibernate 및 JPA 관련 설정

```yaml
spring:
  jpa:
    show-sql: true # 실행된 SQL 쿼리를 콘솔에 출력
    properties:
      hibernate:
        format_sql: true # 보기좋게 출력해줌 (줄바꿈)
    hibernate:
      ddl-auto: create # 어플리케이션을 실행할 때 Hibernate가 DB의 스키마를 자동으로 생성 또는 업데이트를 해줌
```

> 💡 .yaml 파일은 들여쓰기에 매우 민감하기 때문에 주의 해줘야함

## yaml 파일 에서 어플리케이션 설정을 하고 스프링 부트 실행 시켜보기

- application.yaml 파일 코드

```yaml
# 서버 설정
server:
  port: 8080

# 어플리케이션 이름 설정
spring:
  application:
    name: est-service
```

- 스프링부트 실행 코드 작성

```java
package com.example.demo;

import jakarta.annotation.PostConstruct;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Demo1Application {
    @Value("${server.port}") // yaml 파일에서 설정한 포트
    private int port;

    @Value("${spring.application.name}") // yaml 파일에서 설정한 어플리케이션 이름
    private String appName;

    @PostConstruct // 스프링이 실행되고 난 후에 해당 어노테이션이 붙은 메서드 실행
    public void printConfig() {
        System.out.println("포트번호: " + port);
        System.out.println("애플리케이션 이름: " + appName);
    }

    public static void main(String[] args) {
        SpringApplication.run(Demo1Application.class, args);
    }
}
```

- 결과<img src="blog/TIL/7:09/yaml결과.png" alt="#">

## 프로필에 따른 외부 설정

- 스프링 부트 에서는 프로필(profile)을 사용하여 환경에 따라서 다른 설정값을 적용할 수 있습니다.
- 파일명은 `application-(프로필 이름).yaml`과 같은 형식으로 작성 합니다.

### 예시

- 파일명이 application-dev 이고 어플리케이션 이름은 est-service-Dev, 서버 포트는 8081인 설정 파일

```yaml
server:
  port: 8081

spring:
  application:
    name: est-service-Dev
```

- 파일명이 application-prod 이고 어플리케이션 이름은 est-service-prod, 서버 포트는 8082인 설정 파일

```yaml
server:
  port: 8082

spring:
  application:
    name: est-service-Prod
```

- 인텔리제이의 실행/디버그 구성 편집 에서 활성화된 프로파일에 prod 또는 dev 작성<img src="blog/TIL/7:09/profile설정.png" alt="#">
- 결과<img src="blog/TIL/7:09/profile변경결과.png" alt="#">