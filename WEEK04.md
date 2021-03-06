# TIL

UI Interaction Senior, 2020 TIL (Today I Learned)

## 4주차

<!-- 4주 1일차  -->
<details>

<summary>1일차</summary>

## 문서 프로그래밍 인터페이스

### DOM API (Document Object Model API)

자주 사용되는 DOM API

#### DOM 선택 API 메서드

- getElementById
- getElemnetsByTagName()
- getElementsByClassName() (IE 9+)
- querySelector() (IE 8+ CSS2 선택자로 제한, IE 9+)
- querySelectorAll()
- mathches() (IE9+ msMatcheSelctor() )

⚠️ querySelector(), querySelectorAll() 메서드로 수집된 집합은 라이브 상태가 아니다. 문서의 변경내용이 즉각 반영되지 않는다.

#### Node 속성

Node는 HTML을 구성하는 모든것을 말한다. (html요소, 빈줄, 공백 등등)

- childNodes: 특정요소의 직계 자식노드들을 수집 (children: 자식요소 노드들 수집)
- firstChild: 첫번째 자식노드
- lastChild: 마지막 자식노드
- nextSibling: 다음에 인접한 형제노드 (nextElementSibling을 활용하여 다음 요소를 찾자)
- previousSibling: 이전에 인접한 형제노드 (previousElementSibling를 활용하여 이전 요소를 찾자)
- parentNode: 부모 노드
- nodeType
  - Node.ELEMENT_NODE: 1
  - Node.TEXT_NODE: 3
  - Node.CDATA_SECTION_NODE: 4
  - Node.PROCESSING_INSTRUCTION_NODE: 7
  - Node.COMMENT_NODE: 8
  - Node.DOCUMENT_NODE: 9
  - Node.DOCUMENT_TYPE_NODE: 10
  - Node.DOCUMENT_FRAGMENT_NODE: 11
- nodeName: 해당 요소의 태그 이름을 대문자로 반환
- nodeValue: 노드내용을 수집 (textContent를 더 활용 IE9+ 지원 )
</details>
<!-- // 4주 1일차  -->

<!-- 4주 2일차 -->
<details>

<summary>2일차</summary>

## 문서 프로그래밍 인터페이스

### DOM API (Document Object Model API)

- Node.hasChildNodes() : 현재 노드에 자식노드를 가지고 있는지 Boolean 값을 반환한다. (⚠️ 공백이나 주석도 노드이므로 주의!)

```js
var bodyNode = document.querySelector('body');
bodyNode.hasChildNodes(); // true
```

- Document.createElement() : 지정한 tagName의 HTML요소를 생성한다. (tageName을 인식 못할경우 HTMLUnknownElement를 반환한다.)

```js
var divEl = document.createElement('div');
console.log(divEl); // <div></div>
```

- Document.createTextNode(): 텍스트 노드를 생성한다.

```js
var text = document.createTextNode('Text Node 생성'); // Text Node 생성
```

- Node.appendChild(): 특정한 부모노드의 마지막 자식으로 요소를 붙인다.

```js
var scripts = document.createElement('script');
document.body.appendChild(scripts);
```

- Node.insertBefore(): 참조된 노드 앞에 있는 노드에 자식 노드를 삽입한다. (첫번째 전달인자는 삽입하려는 노드, 두번째 전달인자는 삽입 기준이 되는 노드이다. )

```html
<!-- 실행 전 -->
<ul class="parent">
  <li>child</li>
  <li>child</li>
  <li>child</li>
  <li>child</li>
</ul>
```

```js
var parent = document.querySelector('.parent');
var newEl = document.createElement('li');
var newElContent = document.createTextNode('new child');
newEl.appendChild(newElContent);
parent.insertBefore(newEl, parent.firstChild);
```

```html
<!-- 실행 후 -->
<ul class="parent">
  <li>new child</li>
  <li>child</li>
  <li>child</li>
  <li>child</li>
  <li>child</li>
</ul>

<!-- parent.insertBefore(newEl, null); 
두번째 전달인자(기준)가 null 일 경우 요소 맨뒤에 삽입된다.
-->
```

- Element.setAttribute(): 요소에 속성을 추가

```js
document.body.setAttribute('class', 'body-close');
document.body.setAttribute('class', '');
```

- Node.removeChild(): 노드의 자식요소 삭제

```js
var ul = document.createElement('ul');

for (var i = 0, listArray = []; i < 5; i++) {
  listArray.push(document.createElement('li'));
}
listArray.forEach(function (list) {
  ul.appendChild(list);
});
```

- Node.replaceChild() : 특정노드의 자식노드를 다른노드로 교체

```js
var parentNode = document.createElement('span');
var oldContent = document.createTextNode('old old');
parentNode.appendChild(oldContent);

console.log(parentNode); // <span>'old old'</span>

var newContent = document.createTextNode('new new');
parentNode.replaceChild(newContent, oldContent);

console.log(parentNode); // <span>'new new'</span>
```

- Node.cloneNode() 이 메서드를 호출한 노드를 복제하고 반환한다.
  - 인자에 true를 넣을경우 자식 요소까지 복제된다
  - 이벤트까지 복제되지는 않는다.
  - 복제 할 요소에 id 속성이 있을경우 주의하자

```js
var originalNode = document.querySelector('head');
var clone = originalNode.cloneNode(true);

console.log(clone);
```

- CSSStyleDeclaration.cssText: 인라인스타일 설정을 텍스트로 반환
</details>

<!--// 4주 2일차 -->

<!-- 4주 3일차 -->
<details >

<summary>3일차</summary>

## ElementNode 속성 / 메서드

### HTML Element 속성

- ParentNode.children : 읽기전용 속성으로 호출된 요소의 모든 자식노드 중 Elements를 담고 있는 **실시간** HTMLCollection 을 반환 (IE 9이상 지원, 6, 7, 8 도 지원하나 주석노드도 포함)

```html
<ul class="parent">
  <li>children</li>
  <li>children</li>
  <li>children</li>
  <li>children</li>
</ul>

<script>
  var parent = document.querySelector('.parent');
  console.log(parent.children); // HTMLCollection(4) [li, li, li, li]
</script>
```

- ParentNode.firstElementChild: 읽기 전용 속성으로 호출된 요소의 첫번째 Element 자식 반환, 자식요소가 없을 경우 null 반환

```html
<ul class="parent">
  <li class="first">children</li>
  <li>children</li>
  <li>children</li>
  <li>children</li>
</ul>

<script>
  var parent = document.querySelector('.parent');
  console.log(parent.firstElementChild); // <li class="first">children</li>
</script>
```

-ParentNode.lastElementChild: 읽기 전용 속성으로 호출된 요소의 마지막 Element 자식 반환, 자식요소가 없을 경우 null 반환

```html
<ul class="parent">
  <li class="first">children</li>
  <li>children</li>
  <li>children</li>
  <li class="last">children</li>
</ul>

<script>
  var parent = document.querySelector('.parent');
  console.log(parent.lastElementChild); // <li class="last">children</li>
</script>
```

- NonDocumentTypeChildNode.nextElementSibling: 읽기전용속성으로
  호출된 요소의 다음으로 오는 Element 자식 반환

```html
<ul class="parent">
  <li class="first">children</li>
  <li class="second">children</li>
  <li class="third">children</li>
  <li class="fourth">children</li>
</ul>

<script>
  var parent = document.querySelector('.parent');
  console.log(parent.firstElementChild.nextElementSibling); // <li class="second">children</li>
</script>
```

-NonDocumentTypeChildNode.previousElementSibling

```html
<ul class="parent">
  <li class="first">children</li>
  <li class="second">children</li>
  <li class="third">children</li>
  <li class="fourth">children</li>
</ul>

<script>
  var parent = document.querySelector('.parent');
  var second = parent.querySelector('.second');
  console.log(second.previousElementSibling); // <li class="first">children</li>
</script>
```

- element.attributes: 주어진 요소의 속성모음을 실시간 반영하여 반환한다.

```html
<div class="wrap" style="width: 100pxl height: 100px; background: #333"></div>

<script>
  var wrapNode = document.querySelector('.wrap');
  var wrapAttr = wrapNode.attributes;
  console.log(wrapAttr); //   NamedNodeMap {0: class, 1: style}
</script>
```

- innerHTML: 요소 내에 포함된 HTML 마크업을 가져오거나 설정 가능

```html
<ul class="parent">
  <li>first</li>
  <li>second</li>
  <li>third</li>
  <li>fourth</li>
</ul>

<script>
  var parent = document.querySelector('.parent');
  console.log(parent.innerHTML);
  /*
  <li>first</li>
  <li>second</li>
  <li>third</li>
  <li>fourth</li>
  */
  parent.innerHTML = '<li>changed</li>';
  console.log(parent.innerHTML);
  /*
  <li>changed</li>
  */
</script>
```

- outerHTML: 부모요소를 포함한 HTML 마크업을 가져오거나 설정 가능

```html
<ul class="parent">
  <li>first</li>
  <li>second</li>
  <li>third</li>
  <li>fourth</li>
</ul>

<script>
  var parent = document.querySelector('.parent');
  console.log(parent.outerHTML);
  /*
      <ul class="parent">
        <li>first</li>
        <li>second</li>
        <li>third</li>
        <li>fourth</li>
      </ul>
      */
  parent.innerHTML = '<li>changed</li>';
  console.log(parent.outerHTML);
  /*
     <ul class="parent"><li>changed</li></ul>
      */
</script>
```

- HTMLElement.innerText: 요소 내 텍스트 콘텐츠 반환
  - innerHTML은 태그를 해석하므로 script 코드를 악용할 경우 보안에 위협을 줄수 있다.

```html
<p class="paragraph"><strong>HTML</strong> 마크업 내용입니다.</p>

<script>
  var para = document.querySelector('.paragraph');
  para.innerTetxt; // <strong>HTML</strong> 마크업 내용입니다.
</script>
```

- Node.textContent 공백을 모두 포함해서 텍스트 콘텐츠를 반환한다.

  - textContent는 innerHTML이나 innerText 보다 성능 면에서 더 좋을 수 있다.
  - IE 9+ 지원

```html
<p class="paragraph"><strong>HTML</strong> 마크업 내용입니다.</p>

<script>
  var para = document.querySelector('.paragraph');
  para.textContent; // HTML 마크업 내용입니다.
</script>
```

- ParentNode.childElementCount: 자식 엘리먼트 갯수를 반환한다. ( === children.length)

```html
<p class="paragraph"><strong>HTML</strong> 마크업 내용입니다.</p>

<script>
  var para = document.querySelector('.paragraph');
  para.childElementCount; // 1
</script>
```

- Element.classList(ie10+) : class 값이 포함되어있는지 확인하거나 추가, 제거, 토글이 가능하다
  - add() 추가
  - remove() 삭제
  - contains() 포함되어있는지 확인
  - toggle() 토글

* HTMLElement.dataset (IE11+): 개발자 목적의 사용자정의 속성

```html
<div id="user" data-id="1234567890" data-user="johndoe" data-date-of-birth>
  John Doe
</div>
```

```js
const el = document.querySelector('#user');

// getter

// el.id === 'user'
// el.dataset.id === '1234567890'
// el.dataset.user === 'johndoe'
// el.dataset.dateOfBirth === ''

// setter

el.dataset.dateOfBirth = '1960-10-03';
// Result: el.dataset.dateOfBirth === 1960-10-03

// 삭제
delete el.dataset.dateOfBirth;
// Result: el.dataset.dateOfBirth === undefined

// 'someDataAttr' in el.dataset === false
el.dataset.someDataAttr = 'mydata';
// Result: 'someDataAttr' in el.dataset === true
```

### HTML Element 메서드

- getAttribute() : 요소의 속성을 가져온다
- setAttribute() : 요소의 속성을 설정한다.
- removeAttribute(): 요소의 속성을 제거한다.
- hasAttriute(): 요소의 속성을 가지고 있는지 확인

- inserAdjancetHTML(): HTML, XML 텍스트를 파싱하고 특정위치에 원하는 노드를 추가. innrHTML보다 빠르다.

```js
el.insertAdjacentHTML(position, text);

//  position은 아래의 용어만 사용가능
// 'beforebegin' 요소 앞
// 'afterbegin' 요소 안 가장 첫번째 자식
// 'beforeend' 요소 안 가장 미자믹 자식
// 'afterend' 요소 뒤
```

- Element.insertAdjacentElement(): 특정위치에 원하는 요소 노드를 추가

```js
el.insertAdjacentHTML(position, element);
```

</details>
<!--// 4주 3일차 -->

<!-- 4주 4일차 -->

<details open>

<summary>4일차</summary>

## HTML 요소 스타일 속성 / 메서드

- element.style: 요소의 스타일 속성 개체를 반환한다.

  - inline 스타일 속성을 사용하며 별도의 css보다 우선권이 높으므로 주의가 필요하다.
  - 여러개의 요소보다 하나의 요소에 스타일을 설정하는데 주로 쓴다.

```js
$0.style.display('block;)

// 여러번의 스타일 설정하기에는 cssText 속성을 사용하는게 좋다
$0.style.cssText = '\
  margn-top: 20px;\
  font-size: 2rem;\
  '
```

- Window.getComputedStyle() : 인자로 전달받은 요소의 모든 css 속성값을 담은 객체를 반환한다.

```html
<style>
  h3 {
    font-size: 2rem;
    color: orange;
  }
  h3::after {
    content: ' rocks!';
  }
</style>

<h3>generated content</h3>

<script>
  var h3 = document.querySelector('h3');
  var result = getComputedStyle(h3, ':after').content;

  console.log('the generated content is: ', result); // returns ' rocks!'
</script>
```

### CSS 객체 모델(CSSOM)

#### Determining the dimensions of elements

- 전체 공간 크기 속성 (offsetWidth, offsetHeight) : 컨텐츠영역 + 패딩영역 + 보더영역 + 스크롤영역
- 보이는 컨텐츠 크기 (clientWidth, clientHeight) : 컨텐츠영역+ 패딩영역
- 스크롤영역 (scrollWidth, scrollHeight)

#### .getBoundingClientRect() : 요소의 크기 및 뷰포트를 기준으로 한 위치 반환 (IE9+ 지원)

</details>

<!--// 4주 4일차 -->
