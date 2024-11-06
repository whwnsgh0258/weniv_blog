# 스프링 구직 정보 게시판 Post - Domain 및 DTO

[요구사항 명세]("https://whwnsgh0258.github.io/weniv_blog/?post=%5B20240722%5D_%5BSpring+%EA%B2%8C%EC%8B%9C%ED%8C%90+%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%5D_%5BSpring%5D_%5Bproject1.png%5D_%5BSpring%5D_%5B%5D.md")

## 게시판 - Domain

### Post 컬럼 및 연관 관계
- 컬럼
  - id: 게시글의 Id
  - title: 게시글의 제목
  - content: 게시글의 내용
  - user: 게시글을 작성한 사용자의 Id(여러개의 게시글을 하나의 사용자가 작성할 수 있음)
  - isDeleted: 게시글 삭제 여부
  - deletedAt: 게시글 삭제 일시
- 연관관계
  - comments: 게시글에 작성된 댓글

```java

@Entity
@Table(name = "posts")
@Getter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class Post extends BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private Long id;

    @Column(name = "title")
    private String title;

    @Column(name = "content", columnDefinition = "text")
    private String content;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "user_id")
    private User user;

    @OneToMany(mappedBy = "post", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Comment> comments = new ArrayList<>();

    @Column(name = "is_deleted")
    private boolean isDeleted = false;

    @Column(name = "deleted_at")
    private LocalDateTime deletedAt;

    // 연관관계 편의 메소드
    public void addComment(Comment comment) {
        comments.add(comment);
        comment.setPost(this);
    }

    // 게시글 업데이트
    public void updatePost(String title, String content) {
        this.title = title;
        this.content = content;
    }

    public void deletedPost() {
        this.isDeleted = true;
        this.deletedAt = LocalDateTime.now();
    }

    public void setUser(User user) {
        this.user = user;
    }
}
```

## 게시판 - DTO

### Request(요청)

#### CreatePostRequest - 게시글 생성 요청 DTO

```java

@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class CreatePostRequest implements Serializable {

    @NotBlank(message = "제목을 작성해 주세요.")
    @Size(max = 50, message = "제목은 50자 내로 작성해 주세요.")
    private String title;

    @NotBlank(message = "내용을 작성해 주세요.")
    private String content;

    // private User user; userId는 서비스 계층에서 관리
    public Post toEntity() {
        return Post.builder()
                .title(title)
                .content(content)
                .build();
    }
}
```
#### UpdatePostRequest - 게시글 수정 요청 DTO

```java

@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class UpdatePostRequest implements Serializable {

    @NotNull(message = "제목을 입력해 주세요.")
    @Size(message = "제목은 50자 내로 작성해 주세요.", max = 50)
    private String title;

    @NotNull(message = "내용을 작성해 주세요.")
    private String content;
}
```

### Response(응답)

#### PostSummaryResponse - 게시글 요약 DTO
```java

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class PostSummaryResponse implements Serializable {

    private Long id; // 게시글 id
    private String title; // 게시글 제목
    private LocalDateTime createdAt; // 게시글 생성 일시
    private String nickname; // 게시글 작성자
    private String type;

    public static PostSummaryResponse toDTO(Post post) {
        return PostSummaryResponse.builder()
                .id(post.getId())
                .title(post.getTitle())
                .createdAt(post.getCreatedAt())
                .nickname(post.getUser().getNickname())
                .type("post")
                .build();
    }

    public static PostSummaryResponse fromAnnouncement(Announcement announcement) {
        return PostSummaryResponse.builder()
                .id(announcement.getId())
                .title(announcement.getTitle())
                .createdAt(announcement.getCreatedAt())
                .nickname(announcement.getUser().getNickname())
                .type("announcement")
                .build();
    }
}
```

#### PostUserDetailResponse - 게시글 상세 DTO

```java

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class PostUserDetailResponse implements Serializable {

    private Long id; // 게시글 id
    private String title; // 게시글 제목
    private String content; // 게시글 내용
    private List<CommentResponse> commentResponses; // 게시글에 작성된 댓글
    private String nickname; // 게시글 작성자
    private String username; // 게시글 작성자 확인

    public static PostUserDetailResponse toDTO(Post post, List<Comment> comments) {
        List<CommentResponse> commentResponseList = comments.stream()
                .filter(comment -> !comment.getIsDeleted()) // 수정된 부분
                .map(CommentResponse::toDTO)
                .collect(Collectors.toList());
        return PostUserDetailResponse.builder()
                .id(post.getId())
                .title(post.getTitle())
                .content(post.getContent())
                .commentResponses(commentResponseList)
                .nickname(post.getUser().getNickname())
                .username(post.getUser().getUsername())
                .build();
    }

}
```