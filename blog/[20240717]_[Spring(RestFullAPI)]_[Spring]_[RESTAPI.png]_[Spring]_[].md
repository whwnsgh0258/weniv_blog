# @RestController 를 사용해서 API EndPoint 만들기

## @RestController란?

- `@RestController`는 `@Controller` 와 `@ResponseBody` 를 합친 어노테이션 입니다.
- `@RestController` 를 사용하면 해당 컨트롤러 클래스의 모든 메서드에 `@ResponseBody` 가 적용 됩니다.(메서드의 반환값이 뷰 이름이 아닌 실제 데이터로 처리되어 HTTP 응답 본문에 직접 작성)

# DTO, Entity 설계 하기

## DTO vs VO vs DAO

### DTO**(Data Transfer Object)**

- 계층 간 **데이터 전송**을 위한 객체 입니다.(클라이언트와 서버, 서비스 레이어와 컨트롤러 레이어 간의 데이터 전송에 사용됩니다.)
  - Controller Layer 에서 Service Layer 로 데이터 전송
  - Controller Layer 에서 Service Layer 로 응답 데이터 전송
- 비즈니스 로직과 데이터 조작 로직을 포함하지 않습니다.
- **getter**, **setter**, **생성자** 등을 포함하고 **직렬화**를 지원합니다.(json, xml 변환 가능)
- DB의 엔티티에서 **필요한 데이터만 담아서 전송**합니다.

### DAO**(Data Access Object)**

- 데이터 접근 객체로 DB와 상호작용 하는 로직을 캡슐화 합니다.
- DB에 대한 **CRUD** 작업을 수행 합니다.
- 비즈니스 로직을 포함하지 않습니다.
- SQL 쿼리나 DB 연결, 결과 매핑 등의 작업을 처리합니다.

### VO**(Value Object)**

- 값 객체로 불변 객체를 나타냅니다.
- 객체 비교시 동등성(equality)으로 비교합니다.
- 비즈니스 로직을 포함하는 클래스에 작성됩니다.
- 주로 getter만 포함하고 setter는 포함하지 않습니다.
- 도메인 엔티티의 일부 또는 속성 그룹을 나타내는데 사용됩니다.

## 예시
### 게시판의 DTO, Entity 설계
  - BoardPost(게시판 Entity)
      ```java
      @NoArgsConstructor
      @AllArgsConstructor
      @Getter
      @Setter
      public class BoardPost {
              // 게시글의 기본 정보 필드
          private Long id;
          private String title;
          private String content;
          private String author;
          private LocalDateTime createdAt;
          private LocalDateTime updatedAt;
          private List<Comment> comments = new ArrayList<>();
              
              // 댓글 생성
          public void addComment(Comment comment) {
                  // comments 리스트에 작성된 댓글 추가
              comments.add(comment);
              comment.setBoardPost(this);
          }
              
              // 댓글 삭제
          public void removeComment(Comment comment){
                  // comments 리스트에 있는 댓글 삭제
              comments.remove(comment);
              comment.setBoardPost(null);
          }
      }
      ```

  - BoardPostDto(게시판 DTO)
      ```java
      @NoArgsConstructor
      @AllArgsConstructor
      @Getter
      @Setter
      public class BoardPostDto {
              // 전송할 게시글 데이터 필드
          private Long id;
          private String title;
          private String content;
          private String author;
          private LocalDateTime createdAt;
          private LocalDateTime updatedAt;
          private List<CommentDto> comments;
      }
      ```

  - Comment(댓글 Entity)
      ```java
      @AllArgsConstructor
      @NoArgsConstructor
      @Setter
      @Getter
      public class Comment {
              // 댓글 기본 정보 필드
          private Long id;
          private String content;
          private String author;
          private LocalDateTime createdAt;
          private BoardPost boardPost;
      }
      ```

  - CommentDto(댓글 DTO)
      ```java
      @NoArgsConstructor
      @AllArgsConstructor
      @Getter
      @Setter
      public class CommentDto {
              // 전송할 댓글 데이터 필드
          private Long id;
          private String content;
          private String author;
          private LocalDateTime createdAt;
      }
      ```
    
  ### DTO를 활용한 Member 나타내기

  - Member 클래스
    ```java
    @NoArgsConstructor
    @AllArgsConstructor
    @Getter
    @Setter
    public class Member {
        private Long id;
        private String name;
        private String email;
    }
    ```

  - MemberDTO 클래스
    ```java
    @NoArgsConstructor
    @AllArgsConstructor
    @Getter
    @Setter
    public class MemberDTO {
        private String email;
    }
    ```

  - MemberController 클래스
    - 필드
        ```java
        private List<Member> members = new ArrayList<>();
        private long nextId = 1;
        ```

    - READ
        ```java
        // 리소스 전체 조회
        // List화된 Member 클래스의 리소스 전체를 조회
        @GetMapping
        public List<Member> getAllMembers() {
            return members;
        }
        
        // 특정 리소스 조회 메서드
        // ResponseEntity<>를 사용하면 요청 상태 코드를 조작할 수 있습니다.
        // MemberDTO를 반환
        @GetMapping("/{id}")
        public ResponseEntity<MemberDTO> getMemberById(@PathVariable("id") Long id) {
            // members에서 url에 입력된 id와 같은 id를 찾고 없다면 에러를 던짐
            Member member1 = members.stream()
                    .filter(member -> member.getId().equals(id))
                    .findFirst()
                    .orElseThrow(() -> new IllegalArgumentException("에러?"));
          new MemberDTO(member1.getEmail());
          return ResponseEntity.status(404).body(new MemberDTO(member1.getEmail()));
        // 요청을 성공적으로 수행하면 404상태코드 출력(원래는 200)
            }
        ```

    - UPDATE
        ```java
        // 리소스 생성 메서드
        // json, xml 형식으로 리소스를 추가해주는 메서드
        @PostMapping
        public Member createMember(@RequestBody Member member) {
          // 요청을 받으면 id를 1증가시킴
          member.setId(nextId++);
          // 매개변수 member를 members 리스트에 추가시킴
          members.add(member);
          return member;
        }
        ```

    - UPDATE
        ```java
        // 리소스 수정 메서드
            @PutMapping("/{id}")
        public Member updateMember(@PathVariable("id") Long id, @RequestBody Member updateMember) {
          Member member = members.stream()
                  .filter(m -> m.getId().equals(id))
                  .findFirst()
                  .orElseThrow(() -> new IllegalArgumentException("펑펑!"));
            // 변경된 이름과 이메일 적용
          member.setName(updateMember.getName());
          member.setEmail(updateMember.getEmail());
            return member;
        }
        ```

    - DELETE

        ```java
        // 리소스 삭제 메서드
        @DeleteMapping("/{id}")
        public void deleteMember(@PathVariable("id") Long id) {
        members.removeIf(m -> m.getId().equals(id));
        }
        
        ```


# 게시글, 댓글 생성 API 구현

## 게시글 생성

### Entity

- BoardPost
    ```java
    @NoArgsConstructor
    @AllArgsConstructor
    @Getter
    @Setter
    public class BoardPost {
    		// 게시글의 기본 정보 필드
        private Long id;
        private String title;
        private String content;
        private String author;
        private LocalDateTime createdAt;
        private LocalDateTime updatedAt;
        private List<Comment> comments = new ArrayList<>();
    		
    		// 댓글 생성
        public void addComment(Comment comment) {
    		    // comments 리스트에 작성된 댓글 추가
            comments.add(comment);
            comment.setBoardPost(this);
        }
    		
    		// 댓글 삭제
        public void removeComment(Comment comment){
    		    // comments 리스트에 있는 댓글 삭제
            comments.remove(comment);
            comment.setBoardPost(null);
        }
    }
    ```

- Comments
    ```java
    @AllArgsConstructor
    @NoArgsConstructor
    @Setter
    @Getter
    public class Comment {
    		// 댓글 기본 정보 필드
        private Long id;
        private String content;
        private String author;
        private LocalDateTime createdAt;
        private BoardPost boardPost;
    }
    ```


### DTO

- BoardPostDto
    ```java
    @NoArgsConstructor
    @AllArgsConstructor
    @Getter
    @Setter
    public class BoardPostDto {
    		// 전송할 게시글 데이터 필드
        private Long id;
        private String title;
        private String content;
        private String author;
        private LocalDateTime createdAt;
        private LocalDateTime updatedAt;
        private List<CommentDto> comments;
    }
    ```

- CommentsDto
    ```java
    @NoArgsConstructor
    @AllArgsConstructor
    @Getter
    @Setter
    public class CommentDto {
    		// 전송할 댓글 데이터 필드
        private Long id;
        private String content;
        private String author;
        private LocalDateTime createdAt;
    }
    ```


### Controller

- BoardPostController
    ```java
    @RestController
    @RequestMapping("/api/board-post")
    @RequiredArgsConstructor
    public class BoardPostController {
        private final BoardPostService boardPostService;
        
        // 게시글 생성 Controller
        @PostMapping
        public ResponseEntity<BoardPostDto> createBoardPost(@RequestBody BoardPostDto boardPostDTO) {
            BoardPostDto createBoardPostDTO = boardPostService.createBoardPost(boardPostDTO);
            return ResponseEntity.ok(createBoardPostDTO);
        }
    }
    ```


<details>
<summary>💡 <b><code>ResponseEntity<></code>><b> 는 SpringBoot 프레임워크에서 제공하는 클래스로 HTTP 응답을 나타내는 객체 입니다. 해당 클래스를 사용하면 응답 본문, 상태 코드, 헤더 등을 수정할 수 있습니다.</summary>

**역할**
- **응답 본분**: 응답 본분의 데이터 형식을 설정할 수 있습니다.(json, xml, HTML, 문자열 등)
- **HTTP 상태 코드 설정**: 응답 HTTP 상태 코드를 설정할 수 있습니다.(요청이 성공인지 실패인지 만약 실패 했다면 어떤 종류의 에러인지 명확하게 나타낼 수 있습니다.)
- **응답 헤더 설정**: HTTP 응답 헤더를 설정할 수 있습니다.
</details>

### Service

- BoardPostService
    ```java
    @Service
    public class BoardPostService {
        private List<BoardPost> boardPosts = new ArrayList<>();
        private Long nextPostId = 1L;
        private Long nextCommentId = 1L;
    
        // 게시글 생성 메서드
        public BoardPostDto createBoardPost(BoardPostDto boardPostDto) {
            BoardPost boardPost = convertToBoardEntity(boardPostDto);
            boardPost.setId(nextPostId++);
            boardPost.setCreatedAt(LocalDateTime.now());
            boardPosts.add(boardPost);
            return convertToBoardPostDto(boardPost);
        }
    
    		// BoardPost 객체를 BoardPostDto 객체로 변환
        private static BoardPost convertToBoardEntity(BoardPostDto boardPostDto) {
            BoardPost boardPost = new BoardPost();
            boardPost.setTitle(boardPostDto.getTitle());
            boardPost.setContent(boardPostDto.getContent());
            boardPost.setAuthor(boardPostDto.getAuthor());
            if (boardPostDto.getComments() != null) {
                boardPostDto.getComments().forEach(commentDto -> {
                    Comment comment = convertToCommentEntity(commentDto);
                    comment.setBoardPost(boardPost);
                    boardPost.addComment(comment);
    
                });
            }
            return boardPost;
        }
        
    		// 생성된 게시글을 BoardPostDto 객체로 변환하여 반환
        private static BoardPostDto convertToBoardPostDto(BoardPost boardPost) {
            BoardPostDto boardPostDto = new BoardPostDto();
            boardPostDto.setId(boardPost.getId());
            boardPostDto.setTitle(boardPost.getTitle());
            boardPostDto.setContent(boardPost.getContent());
            boardPostDto.setAuthor(boardPost.getAuthor());
            boardPostDto.setCreatedAt(boardPost.getCreatedAt());
            boardPostDto.setUpdatedAt(boardPost.getUpdatedAt());
    
            if (boardPost.getComments() != null) {
                boardPostDto.setComments(
                        boardPost.getComments().stream().map(BoardPostService::convertToCommentDto)
                                .collect(Collectors.toList())
                );
            }
            return boardPostDto;
        }
    ```


### 결과

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e8f11927-b70c-4524-9227-a3efac08e7aa/313e96a0-6c80-4428-8cb0-50ff17f9809c/Untitled.png)

# 회고

수업 시간에 배운 내용을 이해하는데 점점 시간을 많이 쓰게 되는거 같다. 요즘 날도 많이 덥고 습해서 집중력도 떨어지는거 같은데 빨리 정신을 차려서