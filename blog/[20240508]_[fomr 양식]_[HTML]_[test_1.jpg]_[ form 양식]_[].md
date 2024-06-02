# 📄 form 태그


## form이란?

- form 태그는 입력한 데이터를 제출, 전송하기 위해 사용하는 태그이기 때문에 별
    - 사용자에게 입력받은 정보를 제출하기 위한 대화형 컨트롤을 포함하는 문서 구획입니다.

## **`method`** 속성(요청을 보내는 방법)

- 양식을 제출할 때 사용할 HTTP 메서드로 대표적으로 post와 get이 있습니다.

### Post

- 양식 데이터를 요청 본문으로 전송합니다.
- 브라우저에 의해 캐시되지 않고, 브라우저 히스토리에도 남지 않습니다.
- POST 방식의 HTTP 요청에 의한 데이터는 쿼리 문자열과는 별도로 전송 합니다.
- 데이터의 길이 제한이 없고, 브라우저에 캐시 되거나 히스토리에 남지 않기 때문에 GET방식 보다 보안성이 높습니다.
- 주로 데이터를 생성(회원가입, 게시글 생성) 하거나 로그인과 같은 민감한 정보를 다룰 때 사용 됩니다.
- `enctype` 속성 (이부분은 잘 모르겠음*)
    - method 특성이 post인 경우 enctype은 양식 제출 시 데이터의 MIME 타입을 나타냅니다.

        <aside>
        💡 <strong>MIME 타입</strong> 
        - 클라이언트에 전송된 문서의 다양성을 알려주기 위한 메커니즘.
        - 브라우저들은 리소스를 내려받았을 때 해야 할 기본 동작이 무엇인지 결정하기 위해 사용

        </aside>

    - `application/x-www-form-urlencoded`: 기본값
    - `multipart/form-data`: `<input type="file">`이 존재하는 경우 사용

        ```html
        <form 
        	action="http://localhost:8000/" 
        	method="post" 
        	enctype="multipart/form-data"
        >
          <input type="text" name="myTextField">
          <input type="checkbox" name="myCheckBox">Check</input>
          <input type="file" name="myFile">
          <button>Send the file</button>
        </form>
        ```


### Get

- 주로 네이버, 구글, 다음과 같은 검색창에서 많이 사용됩니다.

ex)

![스크린샷 2024-05-08 오후 10.34.35.png](/img/Til/5:8/스크린샷 2024-05-08 오후 10.34.35.png)

- 양식 데이터를 action URL과 ?를 뒤에 붙혀서 전송 합니다.

ex) `https://example.com?name=홍길동&age=20`

(name → name, age) (value → 홍길동, 20)

- GET 방식의 HTTP요청은 **브라우저에 의해 캐시되어 저장 합니다.**
- 쿼리 문자열에 포함되어 전송 되므로 길이제한이 있습니다. (URL 길이제한은 브라우저마다 다릅니다.)
- 보안상 취약점이 존재하므로, 민감한 데이터는 POST 방식을 사용하여 요청 합니다

### name/value

- 이름/값(name/value)의 짝으로 양식과 함께 전송 됩니다.

```html
<!-- 회원가입 예제 -->
<form action="" method="get">
        <label for="user-email">이메일</label>
        <input type="email" name="email" id="user-email">
        <label for="user-pwd">패스워드</label>
        <input type="password" name="pwd" id="user-pwd">

        <fieldset>
            <legend>성별</legend>
            <label for="user-male">남자</label>
            <input type="radio" name="gender" id="user-male" value="male">
            <label for="user-female">여자</label>
            <input type="radio" name="gender" id="user-female" value="female">
            <label for="user-none">선택안함</label>
            <input type="radio" name="gender" id="user-none" value="none">
        </fieldset>

        <fieldset>
            <legend>사용기술</legend>
            <label for="html">HTML</label>
            <input type="checkbox" name="skill" id="html" value="html">
            <label for="css">CSS</label>
            <input type="checkbox" name="skill" id="css" value="css">
            <label for="js">JavaScript</label>
            <input type="checkbox" name="skill" id="js" value="javascript">
        </fieldset>

        <button>회원가입</button>
    </form>
```

- 실행화면

![스크린샷 2024-05-08 오후 10.46.50.png](/img/Til/5:8/0508실습내용.png)

- 회원가입 후 URL
  name → email, pwd, gender, skill
  value → 123%40123, 123, male, html (%40은 인코딩된 ‘@’)

![스크린샷 2024-05-08 오후 10.53.41.png](/img/Til/5:8/url.png)

## `action` 속성 (요청을 처리할 주소)

- 작성된 양식 데이터를 처리할 프로그램의 URL을 적어줍니다.
- 해당 속성을 지정하지 않는다면 form이 있는 페이지의 URL로 보내집니다.

## `autocomplet` 속성 (자동완성)

- 입력요소가 자동완성된 값을 기본값으로 가질 수 있는지 나타냅니다.
- 이전에 해당 양식의 입력된 값이 브라우저의 기억으로 남아있을 경우에 나타납니다.

```html
<input type="email" autocomplete="on" />
```

## label

- 사용자 인터페이스의 항목을 나타냅니다.
- input과 함께 사용해야 합니다
    - 스크린 리더기에서 입력해야 하는 내용이 무엇인지 사용자가 쉽게 이해할수 있게 합니다.
    - label을 클릭하여 input에 포커스를 이동 시키거나 활성화 시킬 수 있습니다.

### for - id를 이용하여 연결

```html
<label for="user-email">이메일</label>
<input type="email" name="email" id="user-email">
```

### label 안에 input 중첩해서 연결하기

```html
<label>
	아이디
	<input type="text">
</label>
```

<aside>
💡 <strong>label 안에 제목요소를 배치하면 안됩니다.</strong> 
- 만약 필요한 경우 `fieldset` `legend` 사용하기!!
</aside>

<aside>
💡 `type = “button”` 선언과 유효한 `value` 속성을 가진 input 요소에는 레이블이 필요하지 않습니다.

</aside>

```html
<input type="button" value="button">
<button type="button">button</button>
```

## button

- 사용자가 클릭할수 있는 요소 입니다.
- form 내부 뿐만 아니라 버튼이 필요한 곳 이라면 어디든 배치 가능합니다.

### button의 타입

- `button`: 기본행동이 없습니다. 클릭 했을 때 아무것도 하지 않고 JavaScript와 연결하여 사용 합니다.
- `submit`: 서버로 양식 데이터를 제출 합니다.
  `<button>___</button>` button 태그에는 기본적으로 submit이 내장되어 있습니다.
- `reset` : 초기값으로 되돌립니다.

### `<a>` vs`<button>` 의 차이점

<a> 태그 특징</a>

1. 마우스 우클릭을 하면, 링크를 새 탭에서 열거나, 링크를 저장하는 등의 추가 옵션이 있는 메뉴가 표시 됩니다.
2. cmd + click과 같은 특수한 기능이 있습니다.
3. 마우스 오버, 포커스가 되어있을 때 이동할 주소를 브라우저 창 하단에 노출 해줍니다.

|  | a | button |
| --- | --- | --- |
| 역할 | 하이퍼링크 | 사용자의 동작 실행을 위한 트리거 |
| 기능 | 다른 페이지 혹은 페이지 내의 특정 영역으로 이동 | 브라우저 기본동작 없음.
JS를 이용하여 동작 추가
(submit: form 전송 / reset: form 초기화) |
| 키보드 | 엔터 | 스페이스, 엔터 |
| 주의 | href 값 없이 JS로 동작하게 하면 안됨! | JS로 동작 |

## input

### 공통 속성

- 주로 type, name, value, placeholder을 많이 사용 합니다

| type | input 양식 컨트롤의 유형 (button, text, email, file…) |
| --- | --- |
| name | input 양식 컨트롤의 이름 |
| value | input 양식 컨트롤의 값 |
| autocomplete | on/off 양식 자동완성 기능에 대한 힌트(check, radio 제외) |
| placeholder | 양식 컨트롤이 비어있을 때 나타나는 내용
- 입력에 대한 힌트 제공 ex) 숫자, 문자 조합의 6자 이상 |
| required | 양식 전송을 위해 필수로 입력해야하는 값 |
| disabled | 비활성화 |
| readonly | 수정불가(읽기전용) |

### 많이 사용하는 input 유형 <input type= >

| button | 버튼. 기본행동 없음. value로 버튼 텍스트 표시 |
| --- | --- |
| submit | 양식 전송 |
| reset | form 내용을 기본값으로 초기화 |
| text | 텍스트 입력 |
| password | 비밀번호 입력(값이 가려짐) |
| email | 이메일 입력 |
| search | 검색 문자열 입력(삭제 아이콘 포함) |
| tel | 전화번호 입력 |
| url | 웹페이지 주소 입력 |
| number | 숫자 입력 |
| checkbox | 단일 값을 선택하거나 선택 해제  |
| radio | 선택 항목중 하나만 선택  |
| file | 파일 업로드 |
| date | 날짜 입력(년,월,일) - 시간 없음 |
| datetime-local | 날짜와 시간을 지정 |
| month | 연과 월 입력 |
| time | 시간 입력 |
| color | 색 선택 |
| range | 슬라이드 바 형태 |
| hidden | 보이지 않지만 값은 서버로 전송하는 컨트롤 |

💡 email, tel, url, number 같은 경우 text로 값을 받아도 되지 않을까?
- type을 통해 어떤 데이터를 받는지 예측 할 수 있으므로 코드의 가독성 향상이 됩니다.
- 모바일에서 type에 따라 키패드 UI가 조금씩 달라지기 때문에 적절한 input type은 사용자 경험 개선에도 도움이 됩니다.

---

### button, reset, submit

- `<button type = "__">` 태그와 동일한 기능을 수행 합니다.

```html
<input input = "button" value = "초기화">
<input input = "reset" value = "초기화">
<input input = "submit" value = "초기화">

<button type = "button">버튼</button>
<button type = "reset">초기화</button>
<button type = "submit">전송</button>
```

- input 태그의 경우 빈 태그 요소이기 때문에 `value` 특정에 텍스트 값 밖에 지정할 수 없습니다.
- button 태그의 경우 여는태그와 닫는 태그 사이에 여러 컨텐츠 삽입이 가능합니다.

### text / password / url / search / tel

- **`maxlength`:** 문자수 최대 길이
- **`minlength`:** 문자수 최소 길이

### checkbox / radio

- **`checkbox`:** 단일 값을 선택하거나 선택 해제 할 수 있는 체크박스
  (여러개를 체크할 수 있음)

```html
<form action="" method="get">
    <label for="color-red">빨강</label>
    <input type="radio" name="color" id="color-red" value="빨강">
    <label for="color-yellow">노랑</label>
    <input type="radio" name="color" id="color-yellow" value="노랑">
    <label for="color-blue">파랑</label>
    <input type="radio" name="color" id="color-blue" value="파랑">
    <button>제출</button>
</form>
```

- **`radrio`:** 단일 값을 선택하거나 선택 해제 할 수 있는 체크박스
  (여러개 체크 불가)

```html
<form action="" method="get">
        <input type="radio" name="gender" id="user-male" value="male">
        <label for="user-female">여자</label>
        <input type="radio" name="gender" id="user-female" value="female">
        <label for="user-none">선택안함</label>
        <input type="radio" name="gender" id="user-none" value="none">
</form>
```

- `checked`: 체크 여부

## file

- 파일을 지정할 수 있습니다.
- `**accept**`: 허용하는 파일 유형을 지정할 수 있습니다.
- `**multiple**`: 지정할 경우 사용자가 여러개의 파일을 선택할 수 있습니다.

```html
<label for="profile">프로필 이미지</label>
<input 
	type="file"
	id="profile"
	name="profile"
  **accept="image/png, image/jpeg"**
>
```

### fieldset

- form 관련 요소들을 묶을 때 사용합니다.
- `disabled` 를 사용할 경우 모든 자손 컨트롤을 비활성화합니다.

### legend

- 그룹의 제목을 제공합니다.

## 회고

### 피드백

- `outocomplete` 가 브라우저에 기록이 남아있을 경우에 자동완성 된 값을 기본값으로 가질 수 있는지 나타내는데 post는 브라우저 히스토리에도 남지 않는다고 나와있어서 “그러면 post를 사용하는 form은 해당 속성을 사용하는게 의미가 없지 않을까?” 라는 생각은 들어서 찾아보니 URL에는 노출되지 않지만 브라우저 기록에는 남아있기 때문에 속성을 사용하는데에 문제가 없다는것을 알게되었습니다.
- 어제와 마찬가지로 HTML 표준에 대해서 정리할 수 있었습니다