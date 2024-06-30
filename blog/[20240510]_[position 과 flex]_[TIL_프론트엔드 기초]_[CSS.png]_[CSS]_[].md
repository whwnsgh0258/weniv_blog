# CSS Box Model

## 마진 병합 현상

1. 요소와 요소의 사이에 `margin-top` 홋은 `margin-bottom` 의 공간이 있을 경우 더 높은 마진 값이 적용되는 현상입니다.
   (block 요소의 경우에만 이러한 현상이 발생 합니다.)
2. 부모 요소와 자식 요소가 존재할 때, 자식 요소의 `margin-top` 혹은 `margin-bottom` 값이 부모의 높이에 영향을 미치지 않는 현상입니다.

## 마진 병합 현상 해결방법

1. 부모요소에 `overflow` 속성값을 적용 합니다.
2. 부모요소에 `display: inlin-block` 값을 적용 합니다
3. 부모 요소에 `border` 값을 적용해줍니다.
4. 부모 요소에 `display:flow-root`을 사용합니다. (형제 요소에는 적용되지 않습니다.)

## overflow, overflow-x, overflow-y

- 박스보다 컨텐츠의 크기가 더 커서 넘치게 될 경우 어떻게 처리할지 결정 합니다.
- overflow는 overflow-x,y의 축약어로 만약 x축 스크롤은 보이게하고, y축 스크롤은 감추고 싶다면 따로 지정해서 작성해야 합니다.
- `visble`: 기본값으로 박스를 넘기는 컨텐츠를 자르지 않고 모두 보여줍니다.
- `hidden`: 요소의 크기를 맞추어서 잘라냅니다.(스크롤 바 미제공)
- `scroll`: 넘치는 크기만큼 잘라내고, 스크롤을 제공 합니다
- `auto`: 자동으로 컨텐츠가 넘칠 경우 스크롤 바를 노출 합니다.

## opacity

- 불투명도를 설정합니다.
- 0-1 사이의 숫자를 지정할 수 있습니다. (0: 투명 - 1: 불투명)
  (0.**과 같이 소숫점 2자리수 까지 사용 가능 하지만 )

---

# form에서 사용하는 가상 선택자

## `:enabled, :disabled`

## `:readonly`

## `:checked`

- input에서 `checkbox`, `radio` 유형일때 선택된 상태로 나타냅니다.

## `:required`

- 반드시 들어가야 하는 곳에 들어갑니다.

## `:placeholder`

- 힌트 줄때

---

# position

## 1. position이란?

말 그대로 HTML내부의 태그들의 위치를 지정 해주는 속성 입니다. position 속성을 이용하여 우리는 페이지의 레이아웃을 결정 할 수 있습니다.

## 2. position의 종류

### 2-1. position: static

모든 태그들은 따로 position 속성을 주지 않았다면 static 값을 가지게 되어서 HTML에 쓴 태그 순으로 흐름에 따라 위치가 지정 됩니다.

- 예시

```HTML
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>static</title>
  <style>
    .box1{
      position:static;
      background-color: green;
      color:white;
      width: 100px;
      height: 100px;
    }

    .box2{
      position:static;
      background-color: red;
      color:white;
      width: 100px;
      height: 100px;
    }

    .box3{
      position:static;
      background-color: blue;
      color:white;
      width: 100px;
      height: 100px;
    }
  </style>
</head>
<body>
  <div class="box1">box1</div>
  <div class="box2">box2</div>
  <div class="box3">box3</div>
</body>
</html>
```

- 결과
  (예시에는 block 요소인 div를 사용 하였기 때문에 아래 와 같은 형태로 나오게 됩니다.)

<img src="blog/TIL/5:10/static.png" alt="스태틱">

### 2.2 position: relative

원래 자신이 있어야 하는 위치에 상대적인 속성을 가지고 있습니다.

- relative 속성은 원래 자리의 자리를 인식 하지만 left, rifht, top, bottom 속성을 이용할 때는 다른 컨텐츠의 레이아웃에 영향을 미치지 않습니다.
- 예시) box1(`postion: static`), box2(`postion: relative`) 상태에서 box2에 `top: 50px;`을 주게되면 기존에 있어야 할 위치에서 왼쪽으로 50px만큼 이동하게 됩니다.
- 결과

<img src="blog/TIL/5:10/relative.png" alt="relative">

### 2.3 position: absolute

absolute는 static을 제외한 position 속성값을 가진 가장 가까운 부모의 박스 내를 기준으로 위치하게 됩니다.

- 한마디로 표현하자면 ‘my way’라고 할 수 있으며 `postion: absolute;`라고 한 뒤 top,right의 속성값을 준다면 우측 상단에 동떨어진 상자 하나를 볼 수 있습니다.
- 예시) box1(`postion: static`), box2(`postion: absolute`) 상태에서 box2에 `top:30px` 을 준다면 box1을 무시하고 최상단에서 30px만큼 떨어져서 나오게 됩니다.
- 결과

<img src="/blog/TIL/5:10/absolute1.png">

- 하지만 absolute는 부모의 위치에 영향을 받기 때문에 부모 태그가 있다면 부모 태그의 위치를 기준으로 움직이게 됩니다
- 예시) box1(부모)을 기준으로

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>CSS Box Model</title>
    <style>
        *{
            margin: 0;
            padding: 0;
            color:white;
        }
        .box1{
            position: relative;
            background-color: green;
            width: 100px;
            height: 100px;
            top: 100px;
        }
        .box2{
            position: absolute;
            top: 30px;
            background-color: blue;
            width: 100px;
            height: 100px;
        }
        .box3{
            position: absolute;
            width: 100px;
            height: 100px;
            background-color: red;
            top: 50px;
            left: 50px;
        }
    </style>
</head>
<body>
<div class="box3">박스3</div>
    <div class="box1">박스1
        <div class="box2">박스2</div>
    </div>
</body>
</html>
```

- 결과

<img src="blog/TIL/5:10/absolute1.png" alt="#">

### 2.4 position: fixed

- view port를 기준으로 우리가 지정한 위치에 고정 됩니다.
- 사용자가 스크롤을 계속 내리더라도 고정되어서 보여주기를 원하는 중요한 정보(주로 상단 카테고리)가 포함된 태그에 사용 합니다.
- 예시) 네이버 검색창
<img src="blog/TIL/5:10/fixed 예시.png" alt="#">

### 2.5 position: sticky

- sticky 속성값이 적용된 요소는 조상에 스크롤이 있다면 가장 가까운 부묘 요소의 컨텐츠 영역에 달라붙는 속성 입니다.
- 예제)

```html
<!doctype html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        section {
            height: 1000px;
            border: 3px solid black;
        }

        h2 {
            position: sticky;
            top: 0px;
            background: greenyellow;
            margin: 0;
        }
    </style>
</head>
<body>
<h1>sticky test</h1>
(section>h2{오늘의 메뉴$}+ul>(li>lorem)*3)*3
</body>
</html>
```

### 2.6 z-index

- postion을 통해 요소의 위치를 변경할때 요소와 요소가 겹쳐 보이는 일이 발생하는데 이때 어떤 요소를 더 먼저 보여줄지 결정할때 사용하는 속성 입니다.
- z-index 값은 크면 클 수록 우선되어서 보여지고, 0을
- 부모가 z-index값을 높여서 자식 앞으로 나올 수 없습니다.
- 자식은 z-index값을 낮추어서 부모 뒤로 가는것은 가능 합니다.
- 자식의 z-index값은 부모의 z-index에 종속 됩니다.(아무리 높아도 부모가 낮으면 의미없음, 하지만 z-index가 없으면 자체 z-index가 적용된다.)

---

# flex

- 브라우저의 개발자 도구에서 flex 속성 변경 가능
- 자기의 직계 자식 에게만 영향을 미칩니다.

## flex-container에 사용하는 속성

- `display:flex`
- 자식 요소들이 컨테이너 안 공간을 맞추기 위해서 크기를 키우거나 줄이는 방법을 설정하는 방법입니다
- 부모 요소를 `flex-container` 자식 요소를 `flex-item` 이라고 분류 합니다.
- 1차원적 레이아웃(x축, 혹은 y축)을 위해 주로 사용합니다.
- `flex-container`의 구조

<img src="blog/TIL/5:10/flex.png" alt="#">

### flex-direction

- 컨테이너 내부의 아이템을 배치할때 주축및 방향을 지정합니다.
- `row`, `column`, `row-reverse`, `column-reverse` 속성값을 사용하여 결정 합니다

### justify-content

- 주축을 기준으로 배열의 위치, 아이템 간의 간격을 설정 할 수 있습니다.
- 주로 `space-between`, `space-around`, `space-evenly` 속성값을 사용하여 설정 합니다.
- 별도로 높이나 넓이를 고정시키지 않으면 부모 요소의 높이나 넓이만큼 늘어납니다.
- 예시) 아래와 같은 구조로 이미지 배치하기

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e8f11927-b70c-4524-9227-a3efac08e7aa/5d938e3b-149a-4768-85df-d2f36a800cc1/Untitled.png)

- 코드

    ```html
    <!doctype html>
    <html lang="ko">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport"
              content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>flex 퀴즈</title>
        <style>
            *{
                margin: 0;
                padding: 0;
            }
            div{
                display: flex;
                justify-content: space-between;
                width: 400px;
                height: 400px;
                background-color: #aaaaaa;
            }
            ul{
                list-style: none;
                display: flex;
                flex-direction: column;
                justify-content: space-between;
            }
            li{
                border: 1px solid black;
                background-color: mediumaquamarine;
                width: 50px;
                height: 50px;
            }
            .box3{
                flex-direction: column-reverse;
            }
        </style>
    </head>
    <body>
        <div>
            <ul class="box1">
                <li>a</li>
                <li>b</li>
                <li>c</li>
            </ul>
            <ul class="box2">
                <li>d</li>
                <li>e</li>
            </ul>
            <ul class="box3">
                <li>f</li>
                <li>g</li>
                <li>h</li>
            </ul>
        </div>
    </body>
    </html>
    ```


### align-items, align-content(한번 더 실습 해보면서 이해가 필요.)

- `align-items`: 교차 축을 기준으로 정렬합니다.(start, center, end를 많이 사용하고 그중 center를 속성값으로 가장 많이 사용 합니다.)
- `align-content`: 컨테이너의 교차 축의 아이템들이 여러 줄일때 사용 가능하다.
    - `flex-wrap:wrap` 인 상태에서 사용해야 한다.

연습문제(**`justify-content: row-reverse 와 end`** 둘다 사용가능)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e8f11927-b70c-4524-9227-a3efac08e7aa/bcad7487-0be3-4138-b5c7-74eeb1d12f18/Untitled.png)

- 코드

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
      <style>
        *{
          margin:0;
          padding:0;
        }
    
        ul{
          display: flex;
          height: 500px;
          width: 500px;
          background-color: royalblue;
          list-style: none;
          /*justify-content: end;*/
            flex-direction: row-reverse;
          align-items: center;
          flex-wrap: wrap;
        }
    
        li{
          width: 50px;
          height: 50px;
          border:1px solid black;
          background-color: teal;
        }
    
      </style>
    </head>
    <body>
    <ul>
      <li>1</li>
    <!--  <li>2</li>-->
    <!--  <li>3</li>-->
    <!--  <li>4</li>-->
    <!--  <li>5</li>-->
    <!--  <li>1</li>-->
    <!--  <li>2</li>-->
    <!--  <li>3</li>-->
    <!--  <li>4</li>-->
    <!--  <li>5</li>-->
    <!--  <li>1</li>-->
    <!--  <li>2</li>-->
    <!--  <li>3</li>-->
    <!--  <li>4</li>-->
    <!--  <li>5</li>-->
    </ul>
    </body>
    </html>
    ```


### gap

- 아이템 간의 간격을 띄우는데에 사용 합니다.
- flex가 적용된 태그에만 적용 됩니다.

### flex-warp

한줄에 배치 할 것인지, 여러줄에 나누어서 사용 할 것인지 결정 합니다.

### flex-flow

- `flex-direction` `flex-warp`의 단축속성 입니다.

# 회고

## 학습방법

1. 강의 시간에 해봤던 실습 내용을 한번 강의가 종료된 후 따로 해보면서 복습하기.
2. 1번을 진행할때 HTML문법을 준수하며 복습하기

## 문제

1. 마진병합 현상에 대해서 이해하는데에 어려움이 있었음.
2. `postion: absolute;` 의 교본을 보며 의미와 작동원리를 이해하는데에 어려움이 있었고, 해당 부분을 실습을 통해 어느정도 해결했음.
3. 기존에는 습관적으로 div와 같은 시멘틱하지 않은 태그를 사용하고 거기에 태그를 붙히는 식으로 작업을 했지만 이런식의 작업은 지양해야 한다고 들어서 의식적으로 사용하지 않으려고 노력 하지만 쉽지않음.

## 앞으로의 목표

1. TIL내용을 작성할 때 조금 더 가독성있게 작성 하도록 노력하기.(남이 봐도 이해하기 쉽게)
2. 결과물이 나온것에 만족하지 않고 코드 한줄 한줄 디테일하게 작성해서 퀄리티 높이기.
3. 이해 안되는 내용은 따로 체크 해두었다가 한번 더 혼자 실습 해보고 깃허브 실습코드를 보며 비교해보기.