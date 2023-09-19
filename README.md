# html css 연습

## 1. 웹페이지 레이아웃
[page](https://ara24.github.io/html-css-practice/1.%EC%9B%B9%ED%8E%98%EC%9D%B4%EC%A7%80%20%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83/html%2Ccss/index.html) , [src](https://github.com/ara24/html-css-practice/blob/main/1.%EC%9B%B9%ED%8E%98%EC%9D%B4%EC%A7%80%20%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83/html%2Ccss/index.html)
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
4. 위의 1~3이 가능하도록 하기 위해서 width가 꼭 있어야 한다. 정해진 길이가 있어야 그것을 넘어가는 것에 대한 처리를 할 수 있다.
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

