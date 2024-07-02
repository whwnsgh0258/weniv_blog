# 데이터 접근 활용 기술

## JPA(ORM) 

ORM(Object Relational Mapping)의 한 형태로, 자바 객체와 데이터베이스 테이블 간의 매핑을 정의하고 관리하는 데 사용

## JDBC API

자바에서 데이터베이스에 접근하고, SQL 쿼리를 실행하며, 결과를 처리하는 표준 인터페이스를 제공하는 API

## Mybatis

객체를 매핑할 때 SQL을 사용하는 기술


# 데이터 접근 기술
## Database In Application
- Java Web Application 에서 Database에 있는 데이터에 접근하기 위해서 JDBC라는
간접적으로 사용
- JDBC는 Java Application 과 데이터베이스를 연결 해주는 API로 JDBC API는 자바 언어로 데이터베이스를 다루기 위한 라이브러리 입니다.

## 스프링이 아닌 Java 환경에서 JDBC 사용해서 데이터 불로오기
### MySQL Connector j 다운받기 및 라이브러리 생성
아래의 그림에서 <b>Platform Independent</b>의 아카이브 파일 다운<br>
<img src="blog/TIL/7:02/MySQL_Connector_j.png" alt="#">

자바 프로젝트에서 라이브러리 폴더를 만들어준 후 <b>mysql-connector-j-9.0.0.jar</b> 파일 옮기기<br> 
<img src="blog/TIL/7:02/lib_file.png" alt="#">

라이브러리 추가

### 직접 데이터 조작 해보기
1. 데이터 생성
```sql
CREATE TABLE test_db.students (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(255) NOT NULL,
  age INT NOT NULL,
  address VARCHAR(255) NOT NULL
);

INSERT INTO test_db.students (name, age, address) VALUES ('오르미', 20, '경기도');
INSERT INTO test_db.students (name, age, address) VALUES ('한빛', 25, '충청도');
```
2. JDBC로 조회하기
```java
public class PlainJdbcExample {
    static final String DB_URL = "jdbc:mysql://localhost:3306/test_db";
    static final String USER = "root";
    static final String PASS = "whwnsgh0258";
    static final String QUERY = "SELECT * FROM students";

    public static void main(String[] args) {
        try(Connection connection = DriverManager.getConnection(DB_URL, USER, PASS)) {
            Statement statement = connection.createStatement();
            ResultSet resultSet = statement.executeQuery(QUERY);

            while (resultSet.next()){
                System.out.println("ID: "+ resultSet.getString("id"));
                System.out.println("Name: "+ resultSet.getString("name"));
                System.out.println("Age: "+ resultSet.getString("age"));
                System.out.println("Address: "+ resultSet.getString("address"));
            }
        } catch (SQLException e) {
            System.out.println(e.getErrorCode());
            System.out.println(e.getMessage());
        } ;

    }
}
```
3. JDBC로 삽입하기
```java
public class InsertPlainJdbcExample {
    static final String DB_URL = "jdbc:mysql://localhost:3306/test_db";
    static final String USER = "root";
    static final String PASS = "whwnsgh0258";
    static final String QUERY = "INSERT INTO students (name,age,address) VALUES (?, ?, ?)";

    public static void main(String[] args) {
        String insertName = "ㅂㅈㄷ";
        int insertAge = 23;
        String insertAddress= "부산";

        String insertName2 = "ㅁㄴㅁ";
        int insertAge2 = 30;
        String insertAddress2= "서울";

        try(Connection connection = DriverManager.getConnection(DB_URL, USER, PASS);
            PreparedStatement preparedStatement = connection.prepareStatement(QUERY)) {
            preparedStatement.setString(1, insertName);
            preparedStatement.setInt(2, insertAge);
            preparedStatement.setString(3, insertAddress);
            int rowsAffected = preparedStatement.executeUpdate();
            System.out.println(rowsAffected + " row(s) updated.");

            preparedStatement.setString(1, insertName2);
            preparedStatement.setInt(2, insertAge2);
            preparedStatement.setString(3, insertAddress2);
            int rowsAffected2 = preparedStatement.executeUpdate();
            System.out.println(rowsAffected2 + " row(s) updated.");


        }catch (SQLException e) {
            System.out.println(e.getErrorCode());
            System.out.println(e.getMessage());
        }
    }
}

```
4. JDBC로 업데이트하기
```java
public class UpdatePlainJdbcExample {
    static final String DB_URL = "jdbc:mysql://localhost:3306/test_db";
    static final String USER = "root";
    static final String PASSWORD = "whwnsgh0258";
    static final String Query = "UPDATE students SET name = ?, age = ?, address = ? WHERE id = ?";
    public static void main(String[] args) {
        try{
            Connection connection = DriverManager.getConnection(DB_URL, USER, PASSWORD);
            PreparedStatement preparedStatement = connection.prepareStatement(Query);
            preparedStatement.setString(1, "John");
            preparedStatement.setInt(2, 25);
            preparedStatement.setString(3, "제주도");
            preparedStatement.setInt(4, 3);

            int rowNum = preparedStatement.executeUpdate();
            System.out.println(rowNum);
        }catch (SQLException e){
            System.out.println(e.getErrorCode());
            System.out.println(e.getMessage());
        }
    }
}
```
5. JDBC로 삭제하기
```java
public class DeletePlainJdbcExample {
    static final String DB_URL = "jdbc:mysql://localhost:3306/test_db";
    static final String USER = "root";
    static final String PASSWORD = "whwnsgh0258";
    static final String Query = "DELETE FROM students WHERE >= ?";
    public static void main(String[] args) {
        // 1. DB connect
        try{
            Connection connection = DriverManager.getConnection(DB_URL, USER, PASSWORD);
            PreparedStatement preparedStatement = connection.prepareStatement(Query);
            preparedStatement.setInt(1, 3);

            int rowNum = preparedStatement.executeUpdate();
            System.out.println(rowNum);
        }catch (SQLException e){
            System.out.println(e.getErrorCode());
            System.out.println(e.getMessage());
        }
        // 2. connect statement
        // 3. 실행
    }
}
```

> <b>Connection</b>: DriverManager.getConnection() 메서드를 사용해서 데이터베이스와 연결해 줍니다.<br>
> <b>(Prepared)Statement</b>: Connect 객체의 CreateStatement() 메서드를 시영히야 SQL문을 실행할 수 있게 해줍니다(Statement 인터페이스는 쿼리에 파라미터를 직접 포함 시켜야 하지만 PreparedStatement 인터페이스는 파라미터를 ?로 지정하여 작성 합니다.)<br>

# Spring 프로젝트에서 JDBC로 데이터 조작하기

## 스프링 프로젝트 생성

- 종속성 추가<br>
<img src="blog/TIL/7:02/dependency.png" alt="#">

- properties 파일 작성
```properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/데이터베이스명
spring.datasource.username=사용자명
spring.datasource.password=비밀번호
```
- domain 폴더에 Student 파일생성 후 데이터 정보 입력
```java
@AllArgsConstructor // 필드에 있는 변수를 파라미터로 가지는 생성자
@NoArgsConstructor // 필드에 있는 변수를 파라미터로 가지지 않는 생성자
@Setter // set 메서드 생성
@Getter // get 메서드 생성
public class Student {
    private teint id;
    private String name;
    private int age;
    private String address;
}
```
- repository 폴더에 인터페이스와 구현체 생성
```java
// 인터페이스
public interface StudentRepository {
    // 데이터 출력 메서드(SELECT)
    List<Student> findAll();
    // 데이터 삽입 메서드(INSERT INTO)
    int insert(Student student);
    // 데이터 삭제 메서드(DELETE)
    int delete(Student student);
}
// 구현체
@Repository
public class StudentRepositoryJdbc implements StudentRepository{
    private JdbcTemplate jdbcTemplate;
    public StudentRepositoryJdbc(JdbcTemplate jdbcTemplate){
        this.jdbcTemplate=jdbcTemplate;
    } 
    // 데이터 출력 메서드 구현
    @Override
    public List<Student> findAll(){
        return jdbcTemplate.qury("SELECT * FROM students", (rs, roNum) -> new Student());
    }
    // 데이터 삽입 메서드
    @Override
    public int insert(Student student){
        return jdbcTemplate.update("INSERT INTO students(name, age, address) VALUES (?,?,?)", 
                student.getName(), student.getAge(), student.getAddress);
    }@Override
    public int delete(Student student){
        return jdbcTemplate.update("DELETE FROM students WHERE id = ?", 
                student.getName(), student.getAge(), student.getAddress);
    }
}
```

- 테스트 코드 작성
```java
// 테스트 코드

@SpringBootTest
class StudentRepositoryJdbcTest {
@Autowired
@Qualifier("studentRepositoryJdbc")
private StudentRepository studentRepository;

    @Test
    void findAll() {
        List<Student> list = studentRepository.findAll();
        for (Student student : list) {
            System.out.print(student.getId());
            System.out.print(student.getName());
            System.out.print(student.getAge());
            System.out.println(student.getAddress());
        }
    }

    @Test
    void insert() {
        Student student = new Student();
        student.setName("장이수");
        student.setAge(40);
        student.setAddress("제주도");
        studentRepository.insert(student);
    }

    @Test
    void delete(){
        Student student = new Student();
        student.setId(1); // 아이디가 1번이 데이터 삭제
        studentRepository.delete(student);
    }
}
```
### 테스트 코드를 작성하고 데이터가 잘 들어갔는지 확인

- 기존 테이블<br>
<img src="blog/TIL/7:02/DataTable.png" alt="#"><br>

- List<Student> findALl() 테스트 실행<br>
<img src="blog/TIL/7:02/List<>findAll().png" alt="#"><br>

- insert 메서드 테스트 실행 후 데이터 확인<br>
<img src="blog/TIL/7:02/insert.png" alt="#"><br>
3번째 아이디에 데이터(장이수, 40, 제주도)가 들어간 것을 확인할 수 있음<br>

- delete 메서드 케스트 실행 후 데이터 확인<br>
<img src="blog/TIL/7:02/delete.png" alt="#"><br>
아이디가 1인 데이터 row가 삭제되고 아이디가 2,3인 데이터 row만 남음

# 회고
-  일단 데이터를 조작하고 테스트 코드를 작성하는것 까지는 할 수 있을거 같음
