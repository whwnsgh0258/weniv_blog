# MyBatis와 JPA

## MyBatis란?

- 자바 객체와 SQL 사이의 자동매핑 기능을 지원하는 ORM(Object Relational Mapping) 프레임워크 입니다.
- JDBC를 통해 RDBMS를 엑세스하는 작업을 캡슐화하고 JDBC 중복 작업을 간소화 시켜 줍니다.
- 개발자가 직접 SQL 쿼리 직접 작성하고 자바 객체와 결과 매핑을 자동으로 처리해 줍니다.

### MyBatis의 기능

1. **SQL 매핑**: 쿼리를 직접 작성하고 이를 매핑 파일이나 어노테이션을 통해 자바 메서드와 연결 해줍니다.
2. **XML및 어노테이션 지원**: SQL 매핑을 위한 XML 설정 파일과 어노테이션을 지원 합니다.
3. **동적 SQL**: `if`, `choose`, `foreach`등의 태그를 사용하여 조건문 등의 작업을 할 수 있습니다.
4. **결과 매핑**: 쿼리의 결과를 자바 객체에 매핑하여 쿼리의 결과를 쉽게 다룰 수 있게 해줍니다

### MyBatis를 사용하는 프로젝트 만들기 (Students 데이터 확인 및 조작)

1. Spring Boot 새 프로젝트 생성
2. 종속송(Lombok, Spring-web, MySQL Driver, Mybatis Framework) 추가
3. properties 파일에 MySQL및 Mybatis 설정

```properties
spring.application.name=Spring-mybatis
# MySQL ??
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/(DB 주소)
spring.datasource.username=(사용자)
spring.datasource.password=(비밀번호)
# Mybatis ??
mybatis.type-aliases-package=(도메인 폴더 경로)
mybatis.configuration.map-underscore-to-camel-case=true
logging.level.com.est.springmybatis.SpringMybatisApplication=trace

```

4. XML 파일 생성

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="매핑할 인터페이스 경로">

</mapper>

```

5. Students 클래스 작성(domain 폴더 )

```java

@Getter
@Setter
// getter, setter 메서드 생성
@NoArgsConstructor
// 파라미터를 받지 않는 생성자 메서드 생성
public class Students {
    private int id;
    private String name;
    private int age;
    private String address;

    public Students(String name, int age, String address) {
        this.name = name;
        this.age = age;
        this.address = address;
    }
}
```

6. xml 폴더와 매핑할 인터페이스 작성(repository 폴더)

```java

@Mapper // xml 폴더와 연결
public interface StudentMapper { // xml 파일의 namespace="매핑할 인터페이스 경로" 과 같은 경로와 파일 이름이어야함

    public int countStudents(); // 학생 수 출력

    List<Students> findStudents(@Param("id") Long id); // 학생 정보 출력

    public saveStudent(Students students); // 학생 추가
}
```

7. StudentController 클래스 작성(controller 폴더)

```java

@RestController
@RequiredArgsConstructor
public class StudentController {
    private final StudentService studentService;

    // url에 localhost:8080/students/count를 작성하면 StudentService 클래스에서 countStudents() 메서드 반환
    @GetMapping("/students/count")
    public int countStudents() {
        return studentService.countStudents();
    }

    // url에 localhost:8080/students를 작성하면 StudentService 클래스에서 getStudents 메서드를 반환
    @GetMapping("/students")
    public List<Students> getAllStudents(@RequestParam(required = false) Long id) {
        return studentService.getStudents(id);
    }

    // POSTMAN 또는 http 파일에서 데이터를 추가하고 정상적으로 작동 되었는지 확인 해주는 메서드
    @PostMapping(value = "/students", produces = MediaType.APPLICATION_JSON_VALUE) // json 형태로 데이터 삽입
    // POST {name:'이름', age:'나이', address='주소'}
    public String addStudent(@RequestBody Students students) {
        studentService.SaveStudent(students);
        return "OK";
    }
}
```

8. StudentService 클래스 작성(service 폴더)

```java

@Service
@RequiredArgsConstructor
public class StudentService {
    private final StudentRepository studentRepository;

    // StudentRepository의 countStudents() 메서드 반환 
    public int countStudents() {
        return studentRepository.countStudents();
    }

    // StudentRepositoryr의 findStudents() 메서드 반환
    public List<Students> getStudents(Long id) {
        return studentRepository.findStudents(id);
    }

    // StudentRepository의 saveStudent 메서드 실행
    public void saveStudent(Students students) {
        studentRepository.saveStudent(students);
    }
}
```

9. StudentRepository 클래스 작성(repository 폴더)

```java

@Repository
@RequiredArgsConstructor // 종속성이 추가된 필드의 생성자 메서드 생성 어노테이션
public class StudentRepository {
    private final StudentMapper studentMapper;

    // StudentMapper의 countStudents() 메서드 반환
    public int countStudents() {
        return studentMapper.countStudents();
    }

    // StudentMapper의 findStudents() 메서드 반환
    public List<Students> findStudents(Long id) {
        return studentMapper.findStudents(id);
    }

    // StudentMapper의 saveStudent 메서드 실행
    public void saveStudent(Students students) {
        studentMapper.saveStudent(students);
    }
    // 학생 추가 메서드만 void인 이유는 insert는 일반적으로 데이터베이스에 데이터가 성공적으로 삽입되었는지 확인하는 것만으로 충분라기 때문
}
```

10. xml 파일에서 쿼리 작성

```xml
학생 수 데이터 조회 쿼리
<select id="countStudents" resultType="int">
    SELECT count(*) FROM students;
</select>

        학생 정보 데이터 조회 쿼리
<select id="findStudents" resultType="Students">
<!--
   Students 클래스로 받은 정보 출력
-->
SELECT id, name, age, address
FROM students
<where>
    아이디가 null 이거나 비어있는 값인 경우를 제외하고 조회
    <if test="id != null and id != ''">
        id = #{id}
    </if>
</where>
</select>

        학생 정보 데이터 삽입 쿼리(데이터 삽입은 어떤 데이터를 삽입 할건지 Postman 또는 http 파일에서 작성 해줘야함)
<insert id="saveStudent" useGeneratedKeys="true" keyProperty="id">
INSERT INTO students (name, age, address)
VALUES (#{name}, #{age}, #{address});
</insert>
```

11. Postman에서 데이터 삽입해주기
    - 우선 Postman을 다운받고 앱에서 로그인<br><img src="blog/TIL/7:03/Postman.png" alt="#">
    - url 작성 및 삽입하고자 하는 데이터 정보 작성<br><img src="blog/TIL/7:03/POST.png" alt="#">
12. 결과 확인
    - 기존데이터<br><img src="blog/TIL/7:03/BeforeData.png" alt="#">
    - 삽입 후 데이터<br><img src="blog/TIL/7:03/AfterData.png" alt="#">

# JPA

## JPA란?

- 자바에서 객체와 RDBMS 간의 매핑을 처리하기 위한 표준 API

## JPA의 장단점

### 장점

- SQL을 직접 작성하지 않고 사용하는 언어로 데이터베이스에 접근할 수 있습니다.
- 객체지향적으로 코드를 작성할 수 있습니다.(비즈니스 로직에 집중할 수 있읍)
- 데이터베이스 시스템이 추상화 되어있습니다.(DBMS 툴을 변경하는 일이 있더라도 내부적으로 조금씩만 바꿔주면 됨)
- 매핑하는 정보가 명확하기 때문에 ERD에 대한 의존도를 낮출 수 있습니다.(DB 설계도를 안보고 자바 코드를 보고 설계도를 예측하여 작업 할 수있음)

### 단점
- 프로젝트가 복잡해 질수록 사용 난이도가 올라갑니다.
- 추상화를 사용하기 때문에 디버깅이 어렵습니다.

## JPA를 활용하여 students 데이터 조작(조회)
1. Spring Boot 새 프로젝트 생성
2. 종속송(Lombok, Spring-web, MySQL Driver, Spring Data JPA) 추가
3. properties 파일에 MySQL및 JPA 설정
```properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/(데이터베이스명)
spring.datasource.username=(사용자)
spring.datasource.password=(비밀번호)

# JPA
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=trace
```
4. Students 클래스 작성(domain 폴더)
```java
@Getter
@Entity // 데이터 베이스와 매핑하는 클래스에 작성해주는 어노테이션
@NoArgsConstructor
public class Students {
    @Id // ID 기본키
    @GeneratedValue(strategy = GenerationType.IDENTITY) // PK 자동 생성
    private Long id;

    @Column // 열 정보
    private String name;

    @Column
    private int age;

    @Column
    private String address;
}

```
5. StudentsRepository 인터페이스 작성
```java
@Repository
public interface StudentRepository extends JpaRepository<Students,Long> {
} // 라이브러리가 제공하는 JpaRepository<> 인터페이스를 상속받음
```
5. StudentsController 클래스 작성(controller 폴더)
```java
// @Controller 는 view를 리턴을 해줌
@RestController // json 파일로 리턴 해줌
@RequiredArgsConstructor
public class StudentsController {
    private final StudentService studentService;

    // 학생 정보 조회
    @GetMapping("/students")
    public List<Students> getAllStudents(){
        return studentService.selectAllStudents();
    }
}
```
6. StudentService 클래스 작성(service 폴더)
```java
@RequiredArgsConstructor // private final 필드 생성자
@Service
public class StudentService {
    private final StudentRepository studentRepository;


    public List<Students> selectAllStudents(){
        return studentRepository.findAll();
    }
}
```
7. 결과 확인<br><img src="blog/TIL/7:03/StudentsData.png" alt="#">

# 회고
1. Mybatis는 수업 할때는 잘 따라갔는데 끝나고 혼자서 다시 해보니까 오류가 뜨는데 다시 코드 뜯어보면서 해볼예정
2. JPA는 또 반대로 수업 할때는 자꾸 오류가 났는데 한번 코드 비교 해보고 다시 작성 해보니까 잘됨(다행...)
3. 어제 오늘 자바에서 DBMS랑 상호작용 하는거 해봤는데 어려움(주말동안 이거 여러번 반복해서 공부 해봐야 할듯)