# 5. CSS3 Transition, Animation

## Transition
css 프로퍼티 값의 상태가 일정 시간동안 변형되도록 하는 속성이다.

예를들어, hover 상태에서 width, height 값이 변경되는 엘리먼트는 마우스를 올린 즉시 크기가 변경된다. trnasition-duration 속성을 2s로 지정하면 2초동안 크기가 변경된다.
```css
.box {
    width: 100px; height: 100px;
    background-color: orange;

    transition-duration: 2s;
}
.box:hover {
    width: 200px; height: 300px;
}
```
```html
<div class="box" />
```

transition은 다음과 같은 속성을 제공한다.
|제목|기본값|내용|
|:---:|:---:|:---:|
|transition||모든 transition 속성을 한번에 사용|
|transition-delay|0s|이벤트 발생 후 몇 초 후에 재생할지 지정|
|transition-duration|0s|몇 초 동안 재생할지 지정|
|transition-property|all|어떤 속성을 변형할지 지정|
|transition-timing-function|ease|수치 변경 함수를 지정|

### transition-duration 속성
변형 지속시간을 설정하는 속성이다.  
예제 코드는 .bar 엘리먼트에 마우스 오버했을 때 width 값의 변경이 3초 동안 진행된다.
```css
.bar {
    width: 10px; height: 50px;
    background-color: orange;

    transition-duration: 5s;
}
.bar:hover {
    width: 600px;
}
```

### transition-timing-function 속성
수치를 변형하는 함수를 지정할 때 사용하고, linear, ease, ease-in, ease-in-out, ease-out이 있다.  
```css
.box {
    transition-timing-function: ease;
}
```
마음에 드는 함수가 없다면 베지어 곡선을 만드는 함수인 cubic-bezier()를 사용할 수 있다.  
[cubic-bezier](https://cubic-bezier.com/)에서 만들 수 있다.
```css
.box {
    transition-timing-function: cubic-bezier(0, 1, 1, 0);
}
```

### transition-property 속성
특정 스타일 속성만 애니메이션을 적용하고 싶을 때 사용한다.
```css
.bar {
    width: 10px; height: 50px;
    background-color: orange;

    transition-property: background-color, width;
    transition-duration: 1s, 5s;
}
.bar:nth-child(1) {
    background-color: red;
    width: 100px;
}
```
.bar 엘리먼트는 background-color는 1초 동안 변형되고, width는 5초 동안 변경된다.  
위의 코드에 정의한 transition 속성을 다음과 같이 한 번에 입력할 수 있다.  
`transition: background-color 1s ease, width 5s linear 1s;`

## Animation과 keyframes
animation은 transition보다 더 복잡한 애니메이션을 나타낼 수 있다.  
transition은 엘리먼트의 변형을 한다면, 애니메이션은 keyframe을 사용해서 퍼센트 단위로 애니메이션을 적용할 수 있고, 여러 개의 keyframe을 중첩 사용하여 보다 더 복잡한 애니메이션을 만들 수 있다.

|제목|기본값|내용|
|:---:|:---:|:---:|
|animation||모든 animation 속성을 한번에 사용|
|animation-delay|0s|이벤트 발생 후 몇 초 후에 재생할지 지정|
|animation-direction|normal|애니메이션 진행 방향을 설정|
|animation-duration|0s|애니메이션을 몇 초 동안 재생할지 지정|
|animation-iteration-count|1|애니메이션 반복 횟수를 지정|
|animation-name||애니메이션 이름을 지정|
|animation-play-state|running|애니메이션 재생 상태를 지정|
|animation-timing-function|ease|수치 변형 함수를 지정|

keyframe은 CSS3 애니메이션을 지정하는 형식이다.  
퍼센트 단위로 적용할 수 있고, 예외적으로 0%는 from, 100%는 to 키워드를 사용할 수 있다.
```css
@keyframes rint {
    from {

    }
    to {

    }
}
```

### animation-name 속성
keyframe을 생성한 이후에는 animation-name 속성을 사용해 엘리먼트를 keyframe에 연결한다.  
```css
#box {
    position: absolute;
    width: 200px; height: 200px;
    border-radius: 100px;
    text-align: center;
    background-color: orange;

    animation-name: rint;
    animation-duration: 2s;
    animation-timing-function: linear;
}
@keyframe rint {
    from { left: 0; transform: rotate(0deg); }
    50% { left: 500px; }
    to { left: 500px; transform: rotate(360px); }
}
```
코드를 실행하면 엘리먼트가 지속시간의 50% 동안은 우측으로 이동하면서 회전하고, 나머지 50% 동안은 제자리에서 회전한다.

**중요!**  
animation-duration 속성을 꼭 지정해야한다.  
애니메이션은 transition과 마찬가지로 지속 시간동안 변현하는 것 이므로, 지속 시간을 지정하지 않으면 동작하지 않는다.

### animation-iteration-count 속성
애니메이션을 특정 횟수만큼 반복하고 싶을 때 사용하는 속성이다.  
무한 반복을 하고 싶을 때는 infinite 키워드를 적용한다.  
`animation-iteration-count: infinite;`

### animation-direction 속성
애니메이션을 반복하는 형태를 지정하는 속성이다.  
- normal: from에서 to로 이동한다.
- alternate: from에서 to로 이동 후 to에서 from으로 이동한다.

`animation-direction: alternate;`

### animation-play-state 속성
애니메이션을 중지하고 재생할 떄 사용하는 속성이다.  
엘리먼트에 마우스를 올렸을 때, 애니메이션을 일시 중지하고 싶으면 다음과 같이 코드를 작성한다.
```css
#box:hover {
    animation-play-state: paused;
}
```

