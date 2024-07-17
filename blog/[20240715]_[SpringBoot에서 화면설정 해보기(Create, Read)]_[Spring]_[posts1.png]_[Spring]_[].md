# 화면 생성(Create, Read) - 게시판 생성하기

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

- PostController 클래스(Create, Read 메서드 추가)
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
}
```

- 화면 생성(게시판 리스트)   
  css파일은 resources 폴더의 static 폴더에 넣어주고 html 파일은 templates 폴더에 넣어줍니다.
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>게시판</title>
    <link rel="stylesheet" th:href="@{/css/style.css}"/>
</head>
<body>
<h1>게시글 목록</h1>
<table>
    <thead>
    <tr>
        <th>번호</th>
        <th>제목</th>
        <th>작성일</th>
    </tr>
    </thead>
    <tbody>
    <tr th:each="post : ${posts}">
        <td th:text="${post.id}"></td>
        <td>
            <a th:href="@{/posts/{id}(id=${post.id})}" th:text="${post.title}"></a>
        </td>
        <td th:text="${post.createdAt}"></td>
    </tr>
    </tbody>
</table>
<a th:href="@{/posts/new}">새 글 작성</a>
</body>
</html>
```

<details>
   <summary><B>CSS</B></summary>

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

- 게시글 리스트 페이지![게시글 리스트](blog/TIL/7:15/PostList.png)
- 글 작성 페이지![게시글 작성 페이지](blog/TIL/7:15/PostCreate.png)
- 작성 후 리스트 페이지(작성된 글이 잘 나오는지 확인)![게시글 작성 후 게시글 목록](blog/TIL/7:15/PostList2.png)]()