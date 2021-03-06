# 3. CSS - FE
========================================

# 3.1 CSS 

## CSS 구성
```
span {
  color : red;
}
```

* span : selector (선택자)
* color : property
* red: value

### style을 HTML페이지에 적용하는 3가지 방법

1. inline

HTML태그 안에다가 적용합니다.
다른 CSS파일에 적용한 것 보다 가장 먼저 적용합니다.

```
<p style="border:1px solid gray;color:red;font-size:2em;">
```

2. internal

style 태그로 지정합니다.
구조와 스타일이 섞이게 되므로 유지보수가 어렵습니다.
별도의 CSS파일을 관리하지 않아도 되며 서버에 CSS파일을 부르기 위해 별도의 브라우저가 요청을 보낼 필요가 없습니다.

```
<head>
<style>
p  {
  font-size : 2em;
  border:1px solid gray;
  color: red;
}
</style>
```

3. external

외부파일(.css)로 지정하는 방식입니다.
CSS 코드가 아주 짧지 않다면 가급적 이 방법으로 구현하는 것이 가장 좋습니다.
현업에서는 여러개의 CSS 파일로 분리하고 이를 합쳐서 서비스에서 사용하기도 합니다.
internal 코드와 같은 css코드를 구현하고, style.css와 같은 별도 파일로 만듭니다. 
이후에 아래처럼 link태그로 추가하면 됩니다.

```
<html>
	<head>
		<link rel="stylesheet" href="style.css">
	</head>
	<body>
		<div>
			<p>
				<ul>
					<li></li>
					<li></li>
					<li></li>
					<li></li>
				</ul>
			</p>
		</div>
	</body>
</html>
```

4. 우선순위 

inline은 별도의 우선순위를 갖지만, internal 과 external은 우선순위가 동등합니다. 따라서 겹치는 선언이 있을 경우 나중에 선언된 속성이 반영됩니다.

### 참고자료
Difference Between Inline, External and Internal CSS Styles - https://www.hostinger.com/tutorials/difference-between-inline-external-and-internal-css


# 3.2 상속과 우선순위 결정

상위에서 적용한 스타일은 하위에도 반영됩니다.
이로 인해 여러 단계로 중첩된 엘리먼트마다 매번 같은 색상과 글자 크기를 부여하지 않아도 됩니다.
하지만 모든 CSS 속성이 이런 특징을 갖게 되면, 몇 가지 문제가 생깁니다.
예를 들어 width 속성이 상속되면 하위 엘리먼트가 모든 같은 크기의 넓잇값을 가질 수 있습니다.
이런 것은 원하는 것이 아니죠.
그래서 box-model이라고 불리는 속성들(width, height, margin, padding, border)과 같이 크기와 배치 관련된 속성들은 하위엘리먼트로 상속이 되지 않습니다.
이렇게 CSS는 꽤 똑똑한 방식으로 동작합니다.  
아직 혼란스러운 부분이 있다면, 여러분들이 중첩된 엘리먼트를 만들고, CSS 속성을 부여하면서 이 특징을 잘 이해해보면 좋습니다.

```
<head>
<style>div { color:red; }</style>
<link rel="stylesheet" href="css.css">
</head>
```
만약 css.css에서 div color를 blue로 주었다면, 뒤에 선언된 external방식의 css내용이 반영됩니다. 따라서 blue색깔로 나오겠죠. 
즉 internal과 external은 같은 우선순위로 결정된다고 아셔야 합니다.
CSS는 여러가지 스타일정보를 기반으로 최종적으로 '경쟁'에 의해서 적절한 스타일이 반영된다는 점을 잘 기억해야 합니다.

```
<div id="a" class="b">
	text....
</div>
```

```
#a{
 color : red;
}

.b{
 color : blue;
}

div{
 color : green;
}
```
위 코드에서 id, class, 엘리먼트 순으로 우선순위를 가집니다.
id는 클래스보다 우선되고 클래스는 엘리먼트보다 우선됩니다.
위 코드에서는 id인 a의 색상이 적용되게 됩니다.
CSS의 이런 성질을 캐스캐이딩이라고 합니다.


## 선언방식에 따른 차이

같은 선택자(selector)라면 나중에 선언한 것이 반영됩니다.
선택자의 표현이 구체적인 것이 있다면 먼저 적용됩니다.

body > span (O)
span (X)

## ID > CLASS > ELEMENT 순으로 반영

만약 h1태그가 div > p > h1 구조로 HTML으로 짜여있다고 가정하면, 아래에 색깔 중 h1이 가진 색깔은 어떤 것일까요?
여러분들이 실험을 통해서 그 결과를 확인해보세요.
jsbin과 유사한 사이트 하나 더 알려드릴게요.
이번에는 codepen.io 라는 사이트를 이용해서 테스트해보세요.

### 참고자료
Specificity - https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity


# 3.3 CSS Selector
특정 엘리먼트에 스타일을 적용하기 위해서는 해당 엘리먼트를 잘 찾아야 합니다.
특정 엘리먼트뿐 아니라 여러 개의 엘리먼트일 수도 있습니다.
엘리먼트를 쉽고 빠르게 찾을 수 있는 것이 CSS Selector 문법입니다. 

## CSS selector

HTML의 요소를 tag, id, html 태그 속성 등을 통해 쉽게 찾아주는 방법입니다.
element에 style 지정을 위한 3가지 기본 선택자

* tag로 지정하기
```
<style>
     span {
       color : red;
     }
</style>
```

* id로 지정하기
```
<style>
     #spantag {
       color : red;
     }
</style>

<body>
     <span id="spantag"> HELLO World! </span>
</body>
```

* class로 지정하기
```
<style>
     .spanClass {
     color : red
     }
</style>

<body>
     <span class="spanClass"> HELLO World! </span>
</body>
```

## CSS Selector의 다양한 활용
* id, class 요소 선택자와 함께 활용
```
#myid { color : red }
div.myclassname { color : red }
```

* 그룹 선택 (여러 개 셀렉터에 같은 style을 적용해야 한다면)
```
h1, span, div { color : red }
h1, span, div#id { color : red }
h1.span, div.classname { color : red }
```

* 요소 선택 (공백) : 자손요소
* 아래 모든 span태그에 red색상이 적용됨
```
<div id="jisu">
  <div>
    <span> span tag </span>
  </div>
  <span> span tag 2 </span>
</div>
```
```
#jisu span { color : red }
```

* 자식 선택 (>) : 자식은 바로 하위엘리먼트를 가리킵니다.
* 아래는 span tag 2만 red 색상이 적용됩니다.
```
<div id="jisu">
  <div>
    <span> span tag </span>
  </div>
  <span> span tag 2 </span>
</div>
```
```
#jisu > span { color : red }
```

* n번째 자식요소를 선택합니다. (nth-child)
* 첫번째 단락에 red 색상이 적용됩니다.
```
<div id="jisu">
  <h2>단락 선택</h2>
  <p>첫번째 단락입니다</p>
  <p>두번째 단락입니다</p>
  <p>세번째 단락입니다</p>
  <p>네번째 단락입니다</p>
</div>
```
```
#jisu > p:nth-child(2) { color : red }
```

### 참고자료
CSS Selectors Cheatsheet - https://gist.github.com/magicznyleszek/809a69dd05e1d5f12d01


# 3.4 CSS 기본 Style 변경하기

CSS style 적용은 글자색, 배경색 등이 가장 자주 사용되는 것들입니다.
이런 속성은 위치 값과 크기를 지정하는 것과 달리, 색상 위주로 값을 부여합니다.
색상 관련 값은 다양한 색을 일관된 방법으로 표현하기 위해 주로 16진수 표기법을 사용합니다.
 
## font 색상 변경

* color : red;
* color : rgba(255, 0, 0, 0.5);
* color : #ff0000;   //16진수 표기법으로 가장 많이 사용되는 방법이죠.

## font 사이즈 변경

* font-size : 16px;
* font-size : 1em;
 
## 배경색 

* background-color : #ff0;
* background-image, position, repeat 등의 속성이 있습니다.
* background : #0000ff url(“.../gif”) no-repeat center top; //한 줄로 정의도 가능
 

## 글씨체/글꼴

* font-family:"Gulim";
* font-family : monospace;

```
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>JS Bin</title>
  <style>
    body > div {
      font-size:16px;
      background-color: #ff0;
      font-family:"Gulim";
    }
    
    .myspan {
      color : #f00;
      font-size:2em;
    }
  </style>
</head>
<body>
  <div>
    <span class="myspan">my text is upper!</span>
  </div>
</body>
</html>
```

## 웹 폰트

웹폰트는 브라우저에서 기본으로 지원하지 않는 폰트를 웹으로부터 다운로드 받아 사용할 수 있는 방법입니다.
다양하고 예쁜 폰트들을 웹폰트로 사용할 수 있긴 하지만 다운로드를 받아야 한다는 단점이 있습니다.
다운로드 시간이 오래 걸리게 되면 화면에 노출되는 시간이 느려져 오히려 사용자에게 불편함을 느끼게 할 수 있는 것 입니다.
또한 다양한 해상도에서 깨지는 문제도 발생할 수 있습니다.
웹폰트는 수많은 종류가 있습니다.
대표적으로 구글 웹폰트가 있으며 최근에는 다양한 크기에서 품질을 유지하는 벡터 방식의 아이콘 웹폰트도 등장했습니다.
(unicode영역 중 Private Use Area (PUA) 영역을 활용해서 제작)
또한 웹폰트 방식말고, 기본 unicode를 사용해서 간단한 아이콘을 표현하는 것도 가능합니다.
아래 unicode를 사용한 HTML 코드를 참고하세요.

```
<div> 안녕하세요 &#x263a;</div> //☺  웹 화면에는 웃음 표시가 표현되는 코드입니다.
```

### 참고자료
bootstrap - https://getbootstrap.com/components/


# 3.5 Element가 배치되는 방법(CSS Layout)

## 엘리먼트가 배치되는 방식

엘리먼트를 화면에 배치하는 것을 layout 작업이라고도 하고, Rendering 과정이라고도 합니다.
편의상 우리는 배치라고 할 겁니다.
기본 엘리먼트는 위에서 아래로 배치되는 것이 기본입니다.
하지만 웹사이트의 배치는 다양하게 표현 가능해야 하므로, 이를 다양한 방식으로 배치할 수 있도록 다양한 속성을 활용합니다.
중요하게 이해해야 할 속성은 다음과 같습니다.

```
display(block, inline, inline-block)
position(static, absolute, relative, fixed)
float(left, right)
```

이 속성을 중심으로 엘리먼트의 배치를 이해할 겁니다.

## 엘리먼트가 배치되는 방식 - (display:block)

display속성이 block이거나 inline-block인 경우 그 엘리먼트는 벽돌을 쌓든 블록을 가지고 쌓입니다.

```
<div>block1</div>
<p>block2</p>
<div>block3</div>
```

## 엘리먼트가 배치되는 방식 - (display:inline)

display 속성이 inline인 경우는 우측으로, 그리고 아래쪽으로 빈자리를 차지하며 흐릅니다.
높이와 넓이를 지정해도 반영이 되지 않습니다.

```
<div>
  <span>나는 어떻게 배치되나요?</span>
  <span>좌우로 배치되는군요</span>
  <a>링크는요?</a>
  <strong>링크도 강조도 모두 좌우로 흐르는군요</strong>
  모두다 display속성이 inline인 엘리먼트이기 때문입니다.
</div>
```
code 바로가기 -  https://jsbin.com/wacukuf/edit?html,output

## 엘리먼트가 배치되는 방식 (position:static, relative, absolute)

엘리먼트 배치가 순서대로만 위아래로 또는 좌우로 흐르면서 쌓이기만 하면, 다양한 배치를 하기 어렵습니다.
position 속성을 사용하면 상대적/절대적으로 어떤 위치에 엘리먼트를 배치하는 것이 수월합니다.
 
1. position 속성으로 특별한 배치를 할 수 있습니다.
position 속성은 기본 static입니다.
그냥 순서대로 배치됩니다.

2. absolute는 기준점에 따라서 특별한 위치에 위치합니다.
top / left / right / bottom 으로 설정합니다.
기준점을 상위엘리먼트로 단계적으로 찾아가는데 static이 아닌 position이 기준점입니다.

3. relative는 원래 자신이 위치해야 할 곳을 기준으로 이동합니다.
top / left / right / bottom로 설정합니다.

4 fixed는 viewport(전체화면) 좌측, 맨 위를 기준으로 동작합니다.
code 바로가기 - https://jsbin.com/vegowut/edit?html,css,output

## 엘리먼트가 배치되는 방식 (margin:10px)

margin으로 배치가 달라질 수 있습니다.
margin은 위 / 아래 / 좌 / 우 엘리먼트와 본인 간의 간격입니다.
따라서 그 간격만큼 내 위치가 달라집니다.

## 엘리먼트가 배치되는 방식 (float:left)

float 속성으로 원래 flow에서 벗어날 수 있고 둥둥 떠다닐 수 있습니다.
일반적인 배치에 따라서 배치된 상태에서 float는 벗어난 형태로 특별히 배치됩니다.
따라서 뒤에 block엘리먼트가 float 된 엘리먼트를 의식하지 못하고 중첩돼서 배치됩니다.

code 바로가기 - https://jsbin.com/cutivij/2/edit?html,css,output

float의 속성은 이런 특이성 때문에 웹사이트의 전체 레이아웃 배치에서 유용하게 활용됩니다.

## 엘리먼트가 배치되는 방식 (box-model)

블록 엘리먼트의 경우 box의 크기와 간격에 관한 속성으로 배치를 추가 결정합니다.
margin, padding, border, outline으로 생성되는 것입니다.

code 바로가기 - https://www.w3schools.com/css/css_boxmodel.asp

box-shadow 속성도 box-model에 포함지어 설명할 수 있습니다.
box-shadow는 border 밖에 테두리를 그릴 수 있는 속성입니다.

## 엘리먼트의 크기

block엘리먼트의 크기는 기본적으로는 부모의 크기만큼을 가집니다.
예를 들어, width:100%는 부모의 크기만큼을 다 갖는 것과 같습니다.


## box-sizing과 padding

padding 속성을 늘리면 엘리먼트의 크기가 달라질 수 있습니다.
box-sizing 속성으로 이를 컨트롤 할 수 있습니다.
box-sizing 속성을 border-box로 설정하면 엘리먼트의 크기를 고정하면서 padding 값만 늘릴 수 있습니다.

code 바로가기 - https://jsbin.com/wosuwop/edit?html,css,output

## layout 구현방법은?

* 전체 레이아웃은 float를 잘 사용해서 2단, 3단 컬럼 배치를 구현합니다.
  최근에는 css-grid나 flex 속성 등 layout을 위한 속성을 사용하기 시작했으며 브라우저 지원범위를 확인해서 사용하도록 합니다.
* 특별한 위치에 배치하기 위해서는 position absolute를 사용하고, 기준점을 relative로 설정합니다.
* 네비게이션과 같은 엘리먼트는 block 엘리먼트를 inline-block으로 변경해서 가로로 배치하기도 합니다.
* 엘리먼트안의 텍스트의 간격과 다른 엘리먼트간의 간격은 padding과 margin 속성을 잘 활용해서 위치시킵니다.


# 3.6 Float 기반 샘플 화면 레이아웃 구성

## 실습코드

html

```
<header>부스트코스는 정말 유익합니다.</header>
<div id="wrap">
   <nav class="left">
      <ul>
         <li>menu</li>
         <li>home</li>
         <li>name</li>
      </ul>
   </nav>
   <div class="right">
      <h2>
         <span>반가워요!</span>
         <div class="emoticon">:-)</div>
      </h2>
   <ul>
      <li>crong</li>
      <li>jk</li>
      <li>honux</li>
      <li>pobi</li>
   </ul>
   </div>
   <div class="realright">
      oh~ right
   </div>
</div>
<footer>코드스쿼드(주)</footer>
```

css

```
li {
   list-style:none;
}

header {
   background-color : #eee;
}

#wrap {
   overflow:auto;
   margin:20px 0px;
}

.left, .right, .realright {
   float:left;
   height: 200px;
}

.left {
   width:17%;
   margin-right:3%;
   background-color : #47c;
}

.right {
   width : 60%;
   text-align:center;
   background-color : #47c;
}

.realright {
   width: 17%;
   margin-left:3%;
   background-color : #67c;
}

.right > h2 {
   position:relative;
}

.right .emoticon {
   position:absolute;
   top:0px;
   right:5%;
   color:#fff;
}

footer {
   background-color : #eee;
   clear:left;
}
```


# 디버깅 - HTML, CSS

크롬 개발자도구의 Element panel을 잘 익혀두는 것이 중요합니다.

개발자도구를 통해서 쉽게 할 수 있는 일들을 정리하면 다음과 같습니다.

* CSS Style을 inline 방식으로 빠르게 테스트할 수 있습니다.
* 현재 엘리먼트의 값을 임시로 바꿀 수 있습니다.
* 최종 결정된 CSS 값을 확인할 수 있습니다.

### 참고자료
페이지와 스타일 검사 및 편집 - https://developer.chrome.com/docs/devtools/css/
