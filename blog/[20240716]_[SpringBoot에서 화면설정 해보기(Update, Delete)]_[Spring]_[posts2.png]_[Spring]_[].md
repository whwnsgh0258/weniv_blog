# 화면 생성(Update, Delete) - 게시판 생성하기

[이전내용](https://whwnsgh0258.github.io/weniv_blog/?post=%5B20240715%5D_%5BSpringBoot%EC%97%90%EC%84%9C+%ED%99%94%EB%A9%B4%EC%84%A4%EC%A0%95+%ED%95%B4%EB%B3%B4%EA%B8%B0%28Create%2C+Read%29%5D_%5BSpring%5D_%5BThymeleaf.png%5D_%5BSpring%5D_%5B%5D.md)

- Post 클래스(Entity 설계)

```java

@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
public class Post {
    private Long id; // PK
    private String title; // 제목
    private String content; // 글 내용
    private LocalDateTime createdAt; // 작성 날짜
}
```

- PostController 클래스(Update, Delete 메서드 추가)

```java

@Controller
@RequestMapping("/posts")
public class PostController {
    private List<Post> posts = new ArrayList<>();
    private Long nextId = 1L;

    @GetMapping
    public String list(Model model) {
        model.addAttribute("posts", posts);
        return "post/list";
    }

    @GetMapping("/new")
    public String newPostModel(Model model) {
        model.addAttribute("post", new Post());
        return "post/form";
    }

    @PostMapping
    public String newPost(@ModelAttribute Post post) {
        post.setId(nextId++);
        post.setCreatedAt(LocalDateTime.now());
        posts.add(post);
        return "redirect:/posts";
    }

    @GetMapping("/{id}") // 게시글 상세 보기 메서드
    public String detail(@PathVariable("id") Long id, Model model) {
        // 게시글 상세 보기 메서드 로직
        Post post = posts.stream()
                .filter(p -> p.getId() == id)
                // id로 필터링(Posts 클래스의 getId가 URL에 입력받은 id가 동일한지 확인)
                .findFirst()
                // List 에서 입력받은 id에 해당하는 첫번째 요소 가져옴
                .orElseThrow(() -> new IllegalArgumentException("게시글이 존재하지 않습니다."));
        // 아니면 에러던짐
        model.addAttribute("post", post);
        return "post/detail";
    }

    @PostMapping("/{id}/delete") // delete 메서드
    public String deletePost(@PathVariable("id") Long id) {
        posts.removeIf(post -> post.getId() == id);
        // 입력받은 id에 해당하는 게시글 삭제
        return "redirect:/posts";
    }

    @GetMapping("/{id}/edit")
    public String editForm(@PathVariable("id") Long id, Model model) {
        Post post = posts.stream()
                .filter(p -> p.getId() == id)
                // id로 필터링(Posts 클래스의 getId가 URL에 입력받은 id가 동일한지 확인)
                .findFirst()
                // List 에서 입력받은 id에 해당하는 첫번째 요소 가져옴
                .orElseThrow(() -> new IllegalArgumentException());
        // 아니면 에러던짐
        model.addAttribute("post", post);
        return "post/edit";
    }

    @PostMapping("/{id}/edit")
    public String editPost(@PathVariable("id") Long id, @ModelAttribute Post updatePost) {
        Post post = posts.stream()
                .filter(p -> p.getId() == id)
                // id로 필터링(Posts 클래스의 getId가 URL에 입력받은 id가 동일한지 확인)
                .findFirst()
                // List 에서 입력받은 id에 해당하는 첫번째 요소 가져옴
                .orElseThrow(() -> new IllegalArgumentException());
        // 아니면 에러던짐
        post.setTitle(updatePost.getTitle());
        post.setContent(updatePost.getContent());
        return "redirect:/posts/{id}";
    }
}
```

<details>
<summary><b>화면 생성</b></summary>
css파일은 resources 폴더의 static 폴더에 넣어주고 html 파일은 templates 폴더에 넣어줍니다.

- html
  - 게시글 상세(details.html)
  ```html
    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
    <head>
      <title>게시글 상세보기</title>
      <link rel="stylesheet" th:href="@{/css/style.css}" />
    </head>
    <body>
    <h1>게시글 상세보기</h1>
    <div class="post-detail">
      <h2 th:text="${post.title}"></h2>
      <p class="post-meta">
          작성일: <span th:text="${post.createdAt}"></span>
      </p>
      <div class="post-actions">
          <a th:href="@{/posts}">목록으로</a>
          <a th:href="@{/posts/{id}/edit(id=${post.id})}" class="edit-btn">수정</a>
          <form th:action="@{/posts/{id}/delete(id=${post.id})}" method="post" class="delete-form">
              <button type="submit">삭제</button>
          </form>
      </div>
    </div>
    </body>
    </html>
  ```
  - 게시글 수정(edit.html)
  ```html
    <!DOCTYPE html>
  <html xmlns:th="http://www.thymeleaf.org">
  <head>
    <title>게시글 수정</title>
    <link rel="stylesheet" th:href="@{/css/style.css}" />
  </head>
  <body>
  <h1>게시글 수정</h1>
  <form th:action="@{/posts/{id}/edit(id=${post.id})}" method="post">
    <div>
      <label for="title">제목</label>
      <input type="text" id="title" name="title" th:value="${post.title}" />
    </div>
    <div>
      <label for="content">내용</label>
      <textarea id="content" name="content" th:text="${post.content}"></textarea>
    </div>
    <button type="submit">저장</button>
  </form>
  </body>
  </html>
  ```
    - **CSS**
    ```css
    /* style.css */
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 20px;
    }
        
    h1 {
      margin-bottom: 20px;
    }
        
    table {
      width: 100%;
      border-collapse: collapse;
    }
        
    table th, table td {
      padding: 10px;
      border: 1px solid #ccc;
    }
        
    table th {
      background-color: #f2f2f2;
      text-align: left;
    }
        
    a {
      display: inline-block;
      margin-top: 20px;
      padding: 10px 20px;
      background-color: #4CAF50;
      color: white;
      text-decoration: none;
      border-radius: 4px;
    }
        
    form {
      max-width: 500px;
      margin: 0 auto;
    }
        
    form div {
      margin-bottom: 20px;
    }
        
    form label {
      display: block;
      font-weight: bold;
      margin-bottom: 5px;
    }
  
    form input[type="text"],
    form textarea {
      width: 100%;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 4px;
    }
        
    form textarea {
      height: 200px;
    }
      
    form button[type="submit"] {
      padding: 10px 20px;
      background-color: #4CAF50;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
    ```

</details>



- 게시글 리스트 페이지![게시글 리스트](blog/TIL/7:16/PostList.png)
- 수정 전 게시글 상세 페이지![수정 전 게시글 상세 페이지](blog/TIL/7:16/postDetails.png)
- 게시글 수정 페이지 ![게시글 수정 페이지](blog/TIL/7:16/postEdit.png)
- 수정 후 게시글 상세 페이지(id가 1인 게시글 수정 후 적용 확인)![게시글 수정 후 상세 페이지](blog/TIL/7:16/editAfter.png)
- 1번 게시글 삭제 후 2번 게시글 생성(id는 기본키이기 때문에 1번 게시글이 삭제 되었어도 id는 2가 나와야함)![삭제 후 2번 게시글 생성](blog/TIL/7:16/delete.png)