# html css 연습

## 목차
1. [웹페이지 레이아웃](#1-웹페이지-레이아웃)
2. [스마트폰 레이아웃](#2-스마트폰-레이아웃)
3. [태블릿 PC 레이아웃](#3-태블릿-pc-레이아웃)
4. [소셜커머스 메인 페이지](#4-소셜커머스-메인-페이지)
5. [CSS3 transition, animation](https://github.com/ara24/html-css-practice/blob/main/5.css3%20transition%2C%20animation/note.md#5-css3-transition-animation)

---

## 1. 웹페이지 레이아웃
[preview](https://ara24.github.io/html-css-practice/1.%EC%9B%B9%ED%8E%98%EC%9D%B4%EC%A7%80%20%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83/html%2Cscss/practice.html) , [src](https://github.com/ara24/html-css-practice/tree/main/1.%EC%9B%B9%ED%8E%98%EC%9D%B4%EC%A7%80%20%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83/html%2Cscss)
- 웹페이지 기본 레이아웃 구성
- 웹 폰트 사용
- 수평 메뉴 제작
- CSS만으로 탭바 구성

### reset css 사용
모든 웹 브라우저에서 동일한 출력 결과를 만들기 위해 reset css를 사용한다.  

#### 초기화 코드 예시
```css
* { margin: 0; padding: 0; }
body { font-family: sans-serif; }
li { list-style: none; }
a { text-decoration: none; }
img { border: 0; }
```
#### 참고
- Eric Meyer's Reset CSS: https://meyerweb.com/eric/tools/css/reset/
- HTML5 Doctor Reset stylesheet: https://html5doctor.com/html-5-reset-stylesheet/

### img 태그 초기화
```html
<a href="#">
    <img src="http://placehold.it/300x200" />
</a>
```

### position:absolute를 적용한  태그의 부모에 height 속성을 강제로 사용한 이유
- 절대위치 좌표는 "특정 크기의 영역"을 지정한 태그 내부에서만 사용할 수 있다.
- 자손의 position 속성에 absolute를 적용하면, 부모에 height 속성이 있어야하고, 부모의 position 속성에 relative 키워드를 적용해야 한다. (부모의 위치를 기준으로 절대위치를 설정하기 때문)

```css
#main_header {
    /* 절대 좌표 */
    height: 160px;
    position: relative;
}

#main_header > #title {
    position: absolute;
    left: 20px; top: 20px;
}

#main_header > #main_gnb {
    position: absolute;
    right: 0; top: 0;
}
```

### 웹 폰트
사용자가 웹 페이지에 접속하는 순간 폰트를 자동으로 내려받고 해당 웹 페이지에서 사용할 수 있게 만들어주는 기능이다.

#### 적용 방법 1 - link 태그에 추가
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Coiny&display=swap" rel="stylesheet">
```
```css
#title {
  font-family: "Coiny", cursive;
}
```
#### 적용 방법 2 - css 영역에서 import
```css
@import url("https://fonts.googleapis.com/css2?family=Coiny&family=Indie+Flower&display=swap");
```
```css
#title {
  font-family: "Indie Flower", cursive;
}
```

### 메뉴 만들 때 a 태그에 block 속성을 사용한 이유
클릭 영역을 확장하기 위해!  

클릭 영역은 a이므로 클릭의 범위가 넓어지도록 하려면 a 태그에 block 속성을 사용해서 영역을 넓혀야 한다.  
만약, li 태그에 padding 속성을 적용해서 버튼 크기를 늘리면 a 태그가 있는 텍스트 영역만 클릭 가능하고, 버튼 안쪽의 여백은 클릭해도 아무 변화가 없게 된다.

### float을 사용하면 부모 요소에 overflow:hidden 사용
원래 float는 이미지를 글자 위에 띄우기 위해 만들어진 스타일 속성이다. 이러한 부유를 막을 때, float 속성을 사용한 부모에 overflow 속성을 사용하고 hidden 키워드를 적용한다.
```css
#content {
    overflow: hidden;
}

#content > #main_section {
    width: 750px;
    float: left;
}

#content > #main_aside {
    width: 200px;
    float: right:
}
```

#### 참고. overflow:hidden으로 해결되는 이유
- 부모 요소는 float된 자식 요소를 고려하지 않는다.
- 최상위 요소인 root(html 요소)는 float된 자식 요소를 인지한다. html은 별다른 처리를 안해도 float 문제가 일어나지 않는 block formatting context(bfc)이기 때문이다.
- html에서 속성을 찾아보니 overflow:hidden이었던 것! 이 속성을 float의 부모에 적용시키면 부모가 작은 block formatting context가 되어 float 높이를 인지할 수 있다.
- overflow의 기본값인 visible 외의 속성일 때 bfc가 생성된다.

### 말줄임 처리
글자를 생략할 때 항상 몰려다니는 삼총사를 기억해둔다. `white-space`, `overflow`, `text-overflow`
```css
strong {
    display: block;
    width: 120px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}
```
1. white-space는 공백을 처리하는 방법을 지정한다. nowrap 속성을 사용하면 연속 공백을 하나로 합치고, 자동 줄바꿈을 제거한다.
2. overflow: hidden을 사용하면 부모 외부로 삐져나온 텍스트를 숨긴다.
3. text-overflow: ellipsis를 하면 텍스트가 잘리는 부분에 말줄임(...)처리를 한다.
4. 위의 1~3이 가능하도록 하기 위해서 width가 꼭 있어야 한다(block 요소여야 한다). 정해진 길이가 있어야 그것을 넘어가는 것에 대한 처리를 할 수 있다.
5. strong 태그가 inline 요소이기 때문에 width를 적용하려면 block요소로 변경해야한다.

### radio 버튼 name
radio 버튼은 name 값이 동일한 것 중에서 하나를 선택한다.
```html
<input id="first" type="radio" name="tab" checked="checked" />
<input id="second" type="radio" name="tab" />
```

### 그 외의 팁
- 클릭 가능한 곳에 a 태그 사용(ex. 각각의 메뉴 등)
- 블록으로 묶어서 정렬해야 하면 `div` 사용
- 주제별로 나눌 수 있다면 `section`으로 작성(ex. 탭1, 탭2)
- 내용이 독립적인 것은 `article`로 작성(ex. 블로그 내용, 뉴스 기사, 포럼 등)
- 엘리먼트가 단 한 개만 있으면 id 사용, 여러 개 있으면 class 사용

### css 적용 우선순위
1. 속성값 뒤의 !important
2. 태그에 inline으로 속성 지정 --- 1000점
3. id 선택 --- 100점
4. class 또는 pseudo class(:hover, :active 등) 선택 --- 10점
5. 태그 이름으로 선택(li, div 같은 것) --- 1점

`div` -> 1점  
`li:first-child` -> 11점  
`ul li` -> 2점  
`ul ol.red` -> 12점  
`#blue` -> 100점

<br/>

---

## 2. 스마트폰 레이아웃
[preview](https://ara24.github.io/html-css-practice/2.%EC%8A%A4%EB%A7%88%ED%8A%B8%ED%8F%B0%20%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83/html%2Cscss/practice.html), [src](https://github.com/ara24/html-css-practice/tree/main/2.%EC%8A%A4%EB%A7%88%ED%8A%B8%ED%8F%B0%20%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83/html%2Cscss)
- 이미지로 그레이디언트 생성
- 스프라이트 이미지 제작
- css만으로 토글 목록 구성
- 스마트폰 화면 전체 채우기

### 뷰 포트 meta 태그
meta 태그는 화면에 특별한 정보를 제공한다.
- width=device-width: 너비를 장치의 너비로 설정
- initial-scale=1.0: 초기 화면 배율을 1배로 설정
- user-scalable=no: 확대 및 축소 불가하도록 설정
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, user-scalable=no">
```

#### 참고. 안드로이드와 아이폰의 meta 태그 설명
- 안드로이드: https://developer.android.com/guide/webapps/targeting?hl=ko
- 아이폰: https://developer.apple.com/library/archive/documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html

### body 안쪽에 div#wrap 추가
전체 컨텐츠의 너비를 조정하기 편리하도록 body 안에 컨텐츠들을 div#wrap으로 감싼다.  
전체적으로 width나 min-width를 적용할 때 활용한다.
```html
<body>
    <div id="wrap">
        ...contents
    </div>
</body>
```

### 이미지로 그레디언트 생성
구버전의 인터넷 익스플로러에서는 CSS3로 그레디언트를 출력할 수 없다.(IE 10부터 지원)  
그래서 서비스한지 오래된 사이트에서는 이미지로 그레디언트를 사용한 것을 볼 수 있다.

![header_background.png](https://ara24.github.io/html-css-practice/2.%EC%8A%A4%EB%A7%88%ED%8A%B8%ED%8F%B0%20%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83/html%2Cscss/header_background.png)

이미지의 높이만큼 요소의 높이를 지정해야 한다. 높이가 가변적인 요소에 적용하기에는 적합하지 않아 보인다.
```css
#main_header {
    /* 이미지의 높이 */
    height: 45px;
    background: url('header_background.png');
}
```

#### 참고  
대부분의 안드로이드 스마트폰은 CSS3 그레이디언트를 지원하지 않는다고 한다.  
오래된 정보일 수 있으니 CSS3 그레이디언트를 적용한다면 이 부분에 대해서 확인이 필요하다.

### 스프라이트 이미지
이미지를 여러 개를 한 개의 파일에 모아놓은 것이다.  
- 장점
    * 파일 개수와 용량이 줄어들어서 웹페이지 요청 시간을 줄일 수 있다.
- 단점
    * 이미지가 추가되면 스프라이트 이미지를 다시 만들어야 하고, 이전에 적용되어있던 설정값들을 모두 바꿔줘야 한다. -> gulp로 자동화할 수 있다.
- 관리 방법
    * 자주 변경되지 않는 이미지들을 스프라이트로 묶어서 관리하고, 자주 변경되는 이미지나 독립된 이미지는 개별 관리한다.

sprite 만드는 무료 사이트: https://www.toptal.com/developers/css/sprite-generator
- Padding between elements(px): 0
- Align element: binary tree

스프라이트를 만들면 사용 방법이 나온다. css에 원하는 이미지의 background-position을 입력해서 사용한다.
```css
#main_header > a.left {
    background: url('sprites.png');
    background-position: 0px 0px;
    /* 들여쓰기. 글자가 화면에 안보이도록 처리 */
    text-indent: -99999px;
}
#main_header > a.left {
    background: url('sprites.png');
    background-position: -62px 0px;
    text-indent: -99999px;
}
```

### 네비게이션 구성 방법 1(overflow, float)
overflow 속성과 float 속성을 사용해 메뉴를 수평 정렬한다.  
4개의 메뉴에 width 값을 25%로 설정하여 동등한 너비를 갖도록 한다.(메뉴의 개수가 고정일 때 사용)  
너비에 소수가 있으면 소수점 둘째 자리까지 적는다.(메뉴 3개이면 width: 33.33%)
```html
<nav id="top_gnb">
    <div><a href="#">버튼</a></div>
    <div><a href="#">버튼</a></div>
    <div><a href="#">버튼</a></div>
    <div><a href="#">버튼</a></div>
</nav>
```
```css
#top_gnb {
    overflow: hidden;
    border-bottom: 1px solid black;
    background: #b42111;
}
#top_gnb > div > a {
    /* 수평 정렬 */
    float: left;
    width: 25%;
    
    /* 크기 및 색상 정렬 */
    height: 35px;
    line-height: 35px;
    text-align: center;
    color: white;
}
```
텍스트 정렬
- `height: 35px; line-height: 35px;`: 수직 가운데 정렬
- `text-align: center`: 수평 가운데 정렬

### 네비게이션 구성 방법 2(display table)
메뉴의 개수가 가변적일 때 사용한다.  
부모에 display: table을 설정하고 자식에 display: table-cell을 설정하면 자식 요소는 동일한 너비가 된다.  
display: table에 너비 지정을 해야 한다.

```html
<nav id="bottom_gnb">
    <div><a href="#">버튼</a></div>
    <div><a href="#">버튼</a></div>
    <div><a href="#">버튼</a></div>
    <div><a href="#">버튼</a></div>
    <div><a href="#">버튼</a></div>
</nav>
```

::before를 사용해서 메뉴 구분선을 만든다.  
구분선에 position: absolute를 지정해서 수평 정렬을 한다. (부모에는 position: relative)
```css
#bottom_gnb {
    display: table;
    /* 너비 지정 */
    width: 100%;
    border-bottom: 1px solid black;
}
#bottom_gnb > div {
    display: table-cell;
    position: relative;
}
#bottom_gnb > div > a {
    display: block;
    height: 35px;
    line-height: 35px;
    text-align: center;
}
/* 클릭 영역 넓히기 */
#bottom_gnb > div > a::before {
    display: block;
    position: absolute;
    top: 9px;
    left: -1px;
    width: 1px;
    height: 15px;
    border-left: 1px solid black;
    content: ' ';
}
```
#### 참고. #bottom_gnb > div > a::before에 적용된 내용에 대해서
- div 태그가 아니라 a 태그에 적용한 이유는 클릭 영역을 넓히기 위함이다.
- ::before에 content 속성은 필수다.
- left: -1px을 적용하여 첫 번째 메뉴의 border-left를 숨긴다.
- 가상 요소와 가상 클래스의 차이
    * 가상 요소는 ::before, ::after 등을 말하고, 콜론이 2개 있다.
    * 가상 클래스는 :hover, :checked 등을 말하고, 콜론이 1개 있다.
    * CSS2에서는 둘 다 콜론이 1개 였지만 CSS3에서 가상클래스와 구분하기 위해 가상 요소는 클론 2개로 변경되었다.

### CSS 속성 선언 순서(우선순위) - 컨벤션이라 회사마다 다를 수 있음
1. display(표시)
2. overflow(넘침)
3. float(흐름)
4. position(위치)
5. width/height(크기)
6. padding/margin(간격)
7. border(테두리)
8. background(배경)
9. color, font(글꼴)
10. animation
11. 기타

### 전체화면 채우기
body 내부의 요소에 height: 100%를 지정했을 때, 화면 전체를 채우려면 html과 body에 height: 100%가 적용되어 있어야 한다.
```css
html, body {
    height: 100%;
}
```

### 페이지 하단에 푸터 고정
페이지에 내용이 부족하면 페이지 하단에 푸터가 노출되고, 내용이 많아서 스크롤이 생겼다면 스크롤을 최하단으로 내렸을 때 하단에 푸터가 노출되도록 설정한다.
```html
<body>
    <div id="wrap">
        <header></header>
        <section></section>
        <footer></footer>
    </div>
</body>
```
```css
html, body { height: 100%; }
#wrap {
    position: relative;
    height: auto;
    min-height: 100%;
    /* 푸터 높이 */
    padding-bottom: 50px;
    box-sizing: border-box;
}
footer {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 50px;
}
```

주의
- 모바일에서 하단에 대화상자 또는 키보드가 있으면 높이가 변경된다. 관련 문제는 자바스크립트의 resize로 해결한다.

### 글자 생략
스마트폰처럼 작은 기계에서 글자가 잘릴 수 있다. 이러한 경우에는 일반적으로 생략 기호를 사용해 생략을 표시한다.
```css
.ellipsis {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}
```
```html
<div class="ellipsis">Lorem ipsum dolor sit amet consectetur, adipisicing elit.</div>
```

## 3. 태블릿 PC 레이아웃
[preview](https://ara24.github.io/html-css-practice/3.%ED%83%9C%EB%B8%94%EB%A6%BF%20PC%20%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83/html%2Cscss/practice.html), [src](https://github.com/ara24/html-css-practice/tree/main/3.%ED%83%9C%EB%B8%94%EB%A6%BF%20PC%20%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83/html%2Cscss)
- 정적 레이아웃과 동적 레이아웃을 조합하는 방법
- 요소 고정

### 화면 너비에 맞게 레이아웃을 동적으로 변화
좌측 정렬된 메뉴와 우측 정렬된 메뉴가 있을 때, 화면 비율에 따라 메뉴 사이의 공간이 줄어들도록 한다.  
화면의 너비가 작아지면서 좌측 메뉴와 우측 메뉴가 겹치면 레이아웃 흐트러지기 때문에 min-height를 설정해야 한다.  
최소 크기를 지정해서 그 이상 줄어들지 않도록 한다.
```html
<nav id="main_gnb">
    <ul class="left">
        <li><a href="#">Button</a></li>
        <li><a href="#">Button</a></li>
        <li><a href="#">Button</a></li>
    </ul>
    <ul class="right">
        <li><a href="#">Button</a></li>
        <li><a href="#">Button</a></li>
    </ul>
</nav>
```
```css
body {
    min-width: 760px;
}

#main_gnb {
    overflow: hidden;
}
#main_gnb > ul.left {
    overflow: hidden;
    float: left;
}
#main_gnb > ul.right {
    overflow: hidden;
    float: right;
}
#main_gnb > ul.left > li { float: left; }
#main_gnb > ul.right > li { float: left; }
```
메뉴 구성을 위해 overflow와 float 세트를 이중으로 사용했다.  
첫 번째는 `#main_gnb` 안에서 메뉴 세트를 각각 left, right로 정렬하기 위함이고, 두 번째는 `ul.left`와 `ul.right` 내부의 버튼들을 left 정렬하기 위해 사용되었다.

### aside(좌측 메뉴) 영역과 section(우측 본문) 영역을 동적 레이아웃으로 적용
좌측 메뉴 영역의 너비를 고정하고, 우측 본문 영역의 너비가 변하도록 한다.
```html
<div id="wrap">
    <nav id="main_lnb">
        ...메뉴들
    </nav>
    <div id="content">
        ...컨텐츠
    </div>
</div>
```
```css
#wrap { overflow: hidden; }
#wrap > #main_lnb {
    float: left;
    width: 200px;
}
#wrap > #content_wrap {
    float: left;
    width: calc(100% - 200px);
}
```
calc을 사용하지 않고, margin과 padding을 이용해서 처리할 수도 있다.
```css
#wrap { overflow: hidden; }
#wrap > #main_lnb {
    float: left;
    width: 200px;
}
#wrap > #content_wrap {
    float: left;
    width: 100%;
    margin-right: -200px;
}
#wrap > #content_wrap > #content {
    padding-right: 200px;
}
```

### 화면에 엘리먼트 고정
position의 fixed 속성을 사용하면 브라우저 화면 기준으로 위치가 결정된다.  
스크롤을 이동시켜도 위치가 변하지 않는다.
```html
<div id="floating_button">Button</div>
```
```css
#floating_button {
    position: fixed;
    bottom: 20px;
    right: 10px;
}
```

## 4. 소셜커머스 메인 페이지
[preview](https://ara24.github.io/html-css-practice/4.%EC%86%8C%EC%85%9C%20%EC%BB%A4%EB%A8%B8%EC%8A%A4%20%EB%A9%94%EC%9D%B8%20%ED%8E%98%EC%9D%B4%EC%A7%80/html%2Cscss/practice.html), [src](https://github.com/ara24/html-css-practice/tree/main/4.%EC%86%8C%EC%85%9C%20%EC%BB%A4%EB%A8%B8%EC%8A%A4%20%EB%A9%94%EC%9D%B8%20%ED%8E%98%EC%9D%B4%EC%A7%80/html%2Cscss)
- 웹 페이지 레이아웃 구성
- 스프라이트 이미지를 사용해서 메뉴 구성

### 사이드 이미지 목록 구성
페이지 우측에 세로로 나열된 이미지는 body 영역 외부에 위치한다.  
inline 요소들은 부모의 너비 안에 가로로 나열되는데 너비가 충분하지 않으면 세로로 나열된다.

```css
body {
    position: relative;
    width: 980px;
    margin: 0 auto;
}
#global_aside {
    position: absolute;
    left: 990px; // 너비보다 더 오른쪽에 위치
    top: 75px;
    width: 90px;
}
#global_aside > a {
    display: block;
    margin-bottom: 5px;
}
```
a 태그에 margin 속성을 적용하기 위해 display 속성에 block 키워드를 적용했다.

### 스프라이트 이미지를 사용해 메뉴 구성
메뉴에 사용할 각각의 이미지들을 스프라이트 이미지로 만들고, 각 메뉴의 엘리먼트에 적용한다.  
기본 이미지와 활성화된 이미지를 포함해서 스프라이트 이미지를 만들면 :hover와 .active를 이용해서 선택된 메뉴와 선택되지 않은 메뉴를 효과적으로 나타낼 수 있다.
```html
<nav id="top_gnb">
    <div class="menu item_1 active"></div>
    <div class="menu item_2"></div>
    <div class="menu item_3"></div>
    <div class="menu item_4"></div>
    <div class="menu item_5"></div>
    <div class="menu item_6"></div>
    <div class="menu item_7"></div>
</nav>
```
```css
#top_gnb {
    overflow: hidden;
}
#top_gnb > div.menu {
    float: left;
    width: 140px;
    height: 50px;
    background: url('Images/top_gnb.png')
}

#top_gnb > div.item_1 { background-position: 0px 0px; }
#top_gnb > div.item_2 { background-position: 0px -50px; }
#top_gnb > div.item_3 { background-position: 0px -100px; }
#top_gnb > div.item_4 { background-position: 0px -150px; }
#top_gnb > div.item_5 { background-position: 0px -200px; }
#top_gnb > div.item_6 { background-position: 0px -250px; }
#top_gnb > div.item_7 { background-position: 0px -300px; }

#top_gnb > div.item_1:hover { background-position: 0px -350px; }
#top_gnb > div.item_2:hover { background-position: 0px -400px; }
#top_gnb > div.item_3:hover { background-position: 0px -450px; }
#top_gnb > div.item_4:hover { background-position: 0px -500px; }
#top_gnb > div.item_5:hover { background-position: 0px -550px; }
#top_gnb > div.item_6:hover { background-position: 0px -600px; }
#top_gnb > div.item_7:hover { background-position: 0px -650px; }

#top_gnb > div.item_1.active { background-position: 0px -350px; }
#top_gnb > div.item_2.active { background-position: 0px -400px; }
#top_gnb > div.item_3.active { background-position: 0px -450px; }
#top_gnb > div.item_4.active { background-position: 0px -500px; }
#top_gnb > div.item_5.active { background-position: 0px -550px; }
#top_gnb > div.item_6.active { background-position: 0px -600px; }
#top_gnb > div.item_7.active { background-position: 0px -650px; }
```

### img 태그를 세로로 나열했을 때 하단에 공간이 생기는 이유와 제거 방법
img 태그의 display 속성은 기본으로 inline이다. img 요소에 담긴 이미지는 텍스트처럼 배치된다. 텍스트가 배치될 때 베이스라인(baseline)을 기준으로 위에 달라붙게 된다. 베이스라인 아래의 여분의 공간은 알파벳 소문자(ex. g, p, y) 등을 표기하기 위해 존재한다.  
diaply 속성을 block으로 변경하면 하단의 공간이 제거된다.
```css
img {
    display: block
}
```
그 외 방법으로는 img 태그에 `vertical-align: bottom;` 또는 `vertical-align: top;` 또는 `line-height: 0;`을 적용하는 방법이 있다.

