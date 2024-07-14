# JPA(3일차)

## JPA가 제공하는 기능

1. **엔티티 테이블 매핑(ORM)**:
    - JPA는 ORM 프레임워크로 자바 객체를 테이블에 매핑 합니다.
    - 엔티티는 테이블에 대응하고, 클래스 필드는 테이블의 컬럼과 대응합니다.
   ```java
   @Entity // students 테이블에 대응
   public class Students{
       
       // 필드
       @Id
       @Column // students 테이블의 컬럼에 대응
       private int id;
   }
   ```
2. **데이터 조작**:
    - JPA는 EntityManager, EntityManagerFactory를 사용해서 데이터를 조작 합니다.
    - CRUD 작업을 위한 기본적인 작업을 할 수 있고, JPQL(Java Persistence Query Language)을 사용해서좀 더 복잡한 작업을 할 수 있습니다.
    - 트랜잭션 관리, 캐싱, 데이터베이스 동기화 작업을 할 수 있습니다.

## Entity 관리: EntityManager, EntityManagerFactory

Spring Boot 에서는 개발자가 직접 EntityManager와 EntityManagerFactory를 만들어서 관리하지 않고 Spring Boot 내부에서 관리 해줍니다.

### EntityManagerFactory

EntityManager를 생성하는 팩토리로 Entity 메니저를 생성하기 위한 설정 정보가 담겨있습니다.

## EntityManager

- JPA에서 실제로 데이터 조작을 담당하는 객체로 CRUD, 쿼리실행, 트랜잭션 관리 등을 수행합니다.
- EntityManager를 통해 영속성 컨텍스트에 저장(persist), 조회(find), 수정(merge), 삭제(remove) 등을 할 수 있습니다.

### JPA에서의 영속성

- 데이터나 객체의 상태를 장기적으로 저장하고 관리하는 환경 입니다.
- 데이터를 메모리에 일시적으로 저장하는것이 아닌 데이터베이스와 같은 영구적인 저장소에 저장하여 어플리케이션이 종료 되어도 데이터가 손실되지 않게 합니다.
  (persist() 메서드는 엔티티 메니저를 사용하여 회원 엔티티를 영속성 컨텍스트 리스트에 저장해 줍니다.)

### 엔티티 생명주기

<img src="https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2Fe8f11927-b70c-4524-9227-a3efac08e7aa%2Ff4472644-7e09-4f88-b122-50105628d113%2FUntitled.png?table=block&id=2a3b8c15-af4a-409d-b24b-61b739de43ef&spaceId=e8f11927-b70c-4524-9227-a3efac08e7aa&width=1470&userId=6780ee7d-d1f0-4161-b474-0dc364611cf7&cache=v2" alt="영속성 생명주기">

1. 비영속 상태(new/Transient)
    - **설명**: 엔티티와 전혀 관계가 없는 상태
    - **특징**: 데이터베이스와 매핑을 하지않은 순수한 객체 입니다.
    - 예시
   ```java
   // 객체를 생성한 상태(비영속)
   Person person = new Person();
   person.setName("John Doe");
   ```
2. 영속 상태(Persistent)
    - **설명**: 엔티티가 영속성 컨텍스트에 의해 관리되고 있는 상태
    - **특징**: 데이터베이스와 동기화 되고 영속성 컨텍스트가 엔티티의 변경을 추적합니다.
    - 예시
   ```java
   // 엔티티를 영속성 컨텍스트에 저장
   em.persist(person);
   ```
3. 준영속상(Detach)
    - **설명**: 기존에 영속성 컨텍스트에서 관리되던 엔티티가 더이상은 관리되지 않는 상태
    - **특징**: 데이터베이스와의 동기화가 끊어진 상태로, 영속성 컨텍스트와 분리된 객체입니다.
    - 예시
   ```java
   // 엔티티를 역속성 컨텍스트에서 분리
   em.detach(person);
   em.close;
   ```
4. 삭제
    - **설명**: 영속성 컨텍스트와 데이터베이스에서 삭제가 이루어질 상태
    - **특징**: 트랜잭션이 커밋되면 데이터베이스에서 삭제 됩니다.
    - 예시
   ```java
   em.remove(person);
   ```

### 영속성 컨텍스트

**엔티티를 영구 저장하는 환경**으로 어플리케이션이 데이터베이스에서 꺼내온 객체를 보관하는 역활을 해줍니다.
(영속성 컨텍스트는 엔티티 메니저를 통해 엔티티를 조회, 저장, 관리하는 역할을 합니다.)

#### 영속성 컨텍스트의 기능

1. **1차 캐시(First Level Cache)**
    - 내부에서 1차 캐시를 가지고 있습니다. 이를 통해 동일한 트랜잭션 내에서 같은 엔티티를 반복 조회할 때 데이터베이스 접근을 최소화 하여 빠른 데이터 조회가 가능 해집니다.
    - EntityManager는 데이터를 조회할 1차 캐시에서 데이터를 조회하고 없으면 데이터베이스에서 데이터를 조회하여 1차 캐시에 저장 합니다.
2. **동일성 보장(Identity Guarantee)**
    - 동일한 엔티티 대한 여러 조회 요청에 동일한 값을 반환합니다.
    - 예시
   ```java
    Person person1 = em.find(Person.class,"Person1"); 
    Person person2 = em.find(Person.class,"Person1");
   
    // person1과 person2는 동일한 엔티티에 대한 요청이므로 반환되는 값은 true 입니다. 
    System.out.println(person1 == person2);
   ```
3. **쓰기 지연(Write-Behind)**
    - 트랜잭션이 커밋 되기 전까지는 데이터베이스에 즉시 반영하지 않고 커밋을 하면 모았던 쿼리를 한번에 전송하여 반영시킵니다.
    - 예시
   ```java
   EntityManager em = emf.createEntityManager();
   EntityTransaction transaction = em.getTransaction();
   
   // 엔티티 메니저는 데이터 변경을 할 때 트랜잭션을 시작해야함
   transaction.begin();
   em.persist(persion1);
   em.persist(persion2);
   // 아직 INSERT SQL을 DB에 보내지않음
   
   // 커밋하면 DB에 위의 INSERT SQL을 보냄
   transaction.commit(); // 커밋 완료 
   ```
   <img src="https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2Fe8f11927-b70c-4524-9227-a3efac08e7aa%2Fa18eb8ec-10fd-45db-ae6a-f5d124d73201%2FUntitled.png?table=block&id=9960a0bd-6c44-4f8b-a639-5b971f83b45d&spaceId=e8f11927-b70c-4524-9227-a3efac08e7aa&width=1470&userId=6780ee7d-d1f0-4161-b474-0dc364611cf7&cache=v2" alt="쓰기 지연"><br>
4. **변경 감지(Dirty Checking)**
    - 객체의 변경사항을 추적하여 트랜잭션이 커밋될 때 자동으로 엔티티를 데이터베이스에 반영합니다.
    - 예시
   ```java
   Person person1 = em.find(Person.class,"Person1");
   person1.setName("이름변경하기")
   // 이러면 자동으로 UPDATE 쿼리가 발생되어 커밋하면 DB의 데이터가 변경됩니다.
   ```
   <img src="https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2Fe8f11927-b70c-4524-9227-a3efac08e7aa%2F2a5511cf-873b-4a67-87de-6a98562fb5a3%2FUntitled.png?table=block&id=10f99976-7cfb-4f19-bcce-a41f0d6b811d&spaceId=e8f11927-b70c-4524-9227-a3efac08e7aa&width=1420&userId=6780ee7d-d1f0-4161-b474-0dc364611cf7&cache=v2" alt="변경 감지">
5. **지연로딩**
    - 쿼리로 요청한 데이터를 바로 어플리케이션에 로딩하는것이 아닌 필요할 때 쿼리를 날려 데이터를 조회하는것 입니다.

# 스프링 데이터 JPA

스프링 데이터 JPA(Spring Data JPA)는 스프링 프레임워크에서 지원하는 중 하나로 원래는 필요한 시점에 커밋을 해줘야 하는것 처럼 개발자가 신경써야 할 부분이 많았지만 라이브러리를 통해 **CRUD를
포함한 여러 기능을 자동으로 쿼리를 만들어서 실행시켜 줍니다.**

## 스프링 데이터 JPA 구성요소

1. **레포지토리 인터페이스**
    - CrudRepository, PagingAndSortingRepository, JpaRepository 등의 인터페이스를 지원합니다.
    - 이는 CRUD 및 페이징 기능을 지원하며 이를통해 개발자는 자신의 레포지토리를 정의할 수 있습니다.
2. **쿼리 메서드**
    - 메서드의 이름을 기반으로 쿼리를 자동으로 생산 합니다.(예 - findByFirstName(String firsName)이라는 메서드를 작성하면 자동으로 firstName 필드로 검색하는 쿼리를 생성)
    - `@Query` 어노테이션을 통해 네이티브 SQL을 작성할 수 있습니다.
3. **페이징 및 정렬 지원**
    - Pageable 인터페이스를 사용하여 간단한 페이징 및 정렬을 구현할 수 있습니다.

### 장정

- 반복적인 코드 감소
    - CRUD 작업을 자동으로 처리 해주기 때문에 반복적인 데이터베이스 접근 코드 작성을 줄일 수 있습니다.
    - 개발자가 인터페이스만 정의 해주면 런타임시 자동으로 구현체가 생성 됩니다.
- 쿼리 메서드
    - 메서드 이름을 기반으로 자동으로 쿼리를 생성하는 기능을 제공합니다.
- 페이징 및 정렬
    - 대용량 데이터를 다룰 때 필요한 페이징과 정렬 기능을 간단하게 구현할 수 있도록 지원합니다.
- 데이터 접근 계층의 일관성
    - 스프링 데이터 JPA는 데이터 접근 계층을 표준화하고 일관성 있게 관리할 수 있도록 도와줍니다.

## 스프링 데이터 JPA에서 제공되는 메서드

### CrudRepository 메서드

- `save(S entity)`: 주어진 엔티티가 존재하면 업데이트하고 그렇지 않으면 삽입하여 저장합니다.(save 뒤에 All을 붙혀 모든 엔티티를 저장할 수 있습니다.)
- `findById(ID id)`: 주어진 id로 엔티티를 조회하고 존재하지 않으면 Optional.Empty()를 반환 합니다.(find 뒤에 All을 붙혀 모든 엔티티를 조회 하거나 By...(
  FirstName, LastName)을 붙혀 특정 조건의 엔티티를 조회 합니다.)
- `existsById()`: 주어진 id의 엔티티가 존재하는지 확인 해줍니다.
- `count()`: 모든 엔티티의 개수를 반환 합니다.
- `delete(T entity)`: 주어진 엔티티를 삭제합니다.(delete 뒤에 All을 붙혀모든 데이터를 삭제하거나 By...(id 등)을 붙형 특정 엔티티를 삭제 할 수 있습니다.)

### PagingAndSortingRepository 메서드

- **CrudRepository**의 모든 메서드를 상속 받습니다.
- `findAll(Sort sort)`: 주어진 정렬 조건에 따라 모든 엔티티를 조회 합니다.
- `findAll(Pageable pageable)`: 주어진 페이징 조건에 따라 모든 엔티티를 조회 합니다.

### JpaRepository 메서드

- **PagingAndSortingRepository**의 모든 메서드를 상속 받습니다.
- `findAll()`: 모든 엔티티를 리스트 형태로 조회 합니다.(매개변수를 Sort를 줘서 정렬 조건에 따라 조회할 수 있고 특정 조건에 따라서 조회 가능 합니다.)
- `saveAllAndFlush(Iterable entities)`: 주어진 엔티티들을 모두 저장하고 플러시 합니다.
- `deleteAllInBatch()`: 모든 엔티티를 삭제 합니다.(메서드에 By...을 작성 해줘서 특정 조건에 해당하는 엔티티를 삭제할 수 있음)
- `GetOne(Id id)`: 특정 아이디를 조회 합니다.(`GetById(Id id)`도 동일)
- `fulsh()`: 모든 변경 사항을 데이터베이스에 플러시 합니다.

# 엔티티 매핑

## 객체와 테이블 매핑
- 테이블과 매핑할 클래스는 `@Entity` 어노테이션을 필수로 붙혀야 합니다.
- 해당 어노테이션이 붙은 클래스는 JPA가 관리하는 테이블이 되고 이 어노테이션을 바탕으로 데이터베이스 스키마를 자동 생성 합니다.

## 기본 키 매핑
- 기본키는 `@id` 어노테이션으로 지정해 줍니다.
- 해당 어노테이션이 지정된 필드는 DB에서의 Primary Key와 동일한 의미로 사용 됩니다.

## 필드와 컬럼 매핑
- `@column`(컬럼 매핑)과 `@Enumerated`(enum 타입 클래스와 매핑), `@Temporal`(날짜 타입 매핑)을 사용하여 컬럼을 매핑할 수 있습니다.
