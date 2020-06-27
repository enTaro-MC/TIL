# TIL

UI Interaction Senior, 2020 TIL (Today I Learned)

## 1주차

<!-- 1주 1일차  -->

<details>
<summary>1일차</summary>

<h3>값 복사 vs 값 참조</h3>

자바스크립트는 데이터 유형에 따라 값이 **복사**되거나 **참조**가 된다

**값이 복사되는 경우 [원시 데이터 유형 (Primitive data type)]**

- null
- undefined
- number
- string
- boolean

검증 방법: 변수의 참조된 값이 변경되었을때 다른 변수의 값이 변하지 않음.

```javascript
  var study = 'Bored';
  var game = 'Funny';
  study = game; game = 'Not funny';
  console.log('study :', study); /* Funny */`
```

**값이 참조되는 경우 (객체 유형 (Object type))**

**가변데이터**의 경우 그값이 **참조**된다

- object
- array
- function

```javascript
var model3 = {
  segment: 'C',
  brand: 'Tesla',
  fuel: 'Electricity',
  zeroToHundred: 4.6,
};

var modelS = model3;
modelS.segment = 'E';

model3 === modelS; // true

console.log('modelS.segemnt :', modelS.segment); // E
console.log('model3.segemnt :', model3.segment); // E

var tesla = ['modelS', 'model3', 'modelX', 'modelY'];
var electricVehicle = tesla;

tesla === electricVehicle; //true

tesla.pop();
console.log(tesla); // ["modelS", "model3", "modelX"]
console.log(electricVehicle); // ["modelS", "model3", "modelX"]

tesla === electricVehicle; //true`
```

가변 데이터도 복사가 가능하다.

```javascript
var model3 = {
  segment: 'C',
  brand: 'Tesla',
  fuel: 'Electricity',
  zeroToHundred: 4.6,
};

var modelY = {};
for(var prop in model3){
  modelY[prop] = model3[prop]
}

console.log(modelY);
model3 ==== modelY  // false

var tesla = ['modelS', 'model3', 'modelX', 'modelY'];

var copyTesla = [];
for(var i=0, l= tesla.length; i< l; i++){
  copyTesla[i] = tesla[i]
}
console.log(copyTesla) //['modelS', 'model3', 'modelX', 'modelY'];
tesla === copyTesla //false

//배열 복사 헬퍼 함수
function copyArray(array){
  for(var copy =[], i = 0, l = array.length; i < l; i++ ){
    copy[i] = array[i]
  }
  return copy;
}
```

### 함수영역 vs 블록영역

**전역 :** 소스코드 상 모든곳에서 사용할 수 있다. 전역 오염(안좋은 습관)
이름공간(name space)을 활용하자!

```javascript
var apple = {};
apple.tablet = 'iPad';
apple.getTablet = function () {
  return this.tablet;
};

apple.getTablet(); // 'iPad'
```

**지역**: 대부분의 프로그래밍 언어의 경우 함수 내에서 유효한 지역변수를 제공

함수영역내에서 var 키워드를 활용하자!
함수 영역내에 해당 키워드가 없을 경우 상위 영역에서 변수를 찾고 상위 영역에도 없을 경우 전역변수를 생성하거나 변수명이 있을경우 해당변수에 값을 참조시켜버린다. (전역오염)

```javascript
// 함수 영역 내에 변수 참조시 var 키워드가 있을 경우

console.log(airpod); // error: airpod is not defiend  변수가 정의되지 않은 상태
var laptop = 'Gram';
function scopeFn() {
  var laptop = 'Mac Book';
  var airpod = 'airpod pro';
}
scopeFn(); //함수 실행

console.log(laptop); //'Gram'
console.log(airpod); // error: airpod is not defiend  변수가 정의되지 않은 상태`
```

```javascript
// 함수 영역 내에 변수 참조시 var 키워드가 없을 경우
console.log(airpod); // error: airpod is not defiend  변수가 정의되지 않은 상태
var laptop = 'Gram';
function scopeFn() {
  laptop = 'Mac Book';
  airpod = 'airpod pro';
}
scopeFn(); //함수 실행

console.log(laptop); //'Mac Book'
console.log(airpod); // airpod pro (전역에 변수 생성해버림)`
```

자바스크립트는 블록영역을 지원하지 않는다. 단 ES6 부터 let 키워드는 블록 영역을 지원한다.

```javascript
var script = 'Javascript';
console.log('전역 스크립트는 ' + script + ' 입니다');

if (true) {
  var script = 'ECMAScript';
  console.log('블록 스크립트는 ' + script + ' 입니다');
}

console.log(script); //ECMAScript`
```

```javascript
var script = 'Javascript';
console.log('전역 스크립트는 ' + script + ' 입니다');

if (true) {
  let script = 'ECMAScript';
  console.log('블록 스크립트는 ' + script + ' 입니다');
}

console.log(script); //Javascript`
```

### 호이스팅

자바스크립트엔진은 변수의 선언부분과 함수의 선언부분을 컴퓨터 메모리에 우선 상주시킨다.

변수는 변수 선언 -> 변수 값을 할당하는 초기화 과정 -> 변수 스코프를 설정하는 과정이 순차적으로 이루어짐

```javascript
// 변수 초기화
hoist_is = [
  '컴파일 단계에서 메모리에 저장',
  '초기화가 아닌, 선언만 호이스트',
  'JavaScript에서 컨텍스트 실행이 작동하는 방식',
];

// 변수 초기화, 함수 실행
cloned_arr = copyArray(hoist_is);

// 콘솔 출력
console.log(cloned_arr);

// 함수 선언
function copyArray(data) {
  if (!clone && Array.isArray(data)) {
    for (var clone = [], i = 0, l = data.length; i < l; ++i) {
      clone.push(data[i]);
    }
    return clone;
  } else {
    return [];
  }
}

// 변수 선언
var cloned_arr, hoist_is;
```

```javascript
//변수 선언
var cloned_arr, hoist_is;
//함수 선언
function copyArray(data) {
  var clone, i, l;
  if (!clone && Array.isArray(data)) {
    for (clone = [], i = 0, l = data.length; i < l; ++i) {
      clone.push(data[i]);
    }
    return clone;
  } else {
    return [];
  }
}

// 변수 초기화
hoist_is = [
  '컴파일 단계에서 메모리에 저장',
  '초기화가 아닌, 선언만 호이스트',
  'JavaScript에서 컨텍스트 실행이 작동하는 방식',
];

// 변수 초기화, 함수 실행
cloned_arr = copyArray(hoist_is);

// 콘솔 출력
console.log(cloned_arr);
```

함수의 경우 현재 영역 내에 값을 못찾을경우 한단계 위에서 찾으려 한다 (스코프 체이닝)

- 성능관점에서 좋지 못하다.

※ 변수 선언/초기화, 함수 선언/표현식을 상위에 영역 내 최상위에 작성하는 습관을 들이자! 그 이후에 변수 및 함수실행문 작성

### 즉시실행함수시

가급적이면 전역을 오염시키지 위한 방법으로 IIFE 패턴을 활용한다.

```javascript
var fn1 = function () {
  return 'function1';
};
// fn1 = function
fn1(); // 'function1'

// 즉시실행함수
var fn2 = (function () {
  return 'function2';
})();
fn2; // 'function2'
fn2(); // fn2 is not function`
```

</details>

<!-- // 1주 1일차  -->

<!-- 1주 2일차  -->

<details>

<summary>2일차</summary>

### ES6 변수와 상수

**Javascript의 변수 선언**

```javascript
// 변수 선언 (declaration)
var declaration; //var 키워드로 변수 선언

// 변수 초기화 (initialization)
declaration = '선언된 변수에 값을 할당한다';

// 범위 설정 (scope)
프로그램 내부에서 접근가능한 영역 설정
전역(Global), 함수(Function) 영역을 가짐
함수 내부에서 선언한 변수는 함수(지역)내부에서만 접근 가능


//함수 선언
function getDate(){
  //함수 내부에 변수 선언 및 초기화 (함수 범위 설정)
  var date = new Date();
  return date;
}

//함수실행
getDate();

console.log(date)
// 함수영역 내에 선언된 변수는 외부에서 접근 불가하므로 오류발생
// Uncaught ReferenceError: date is not defined at <anonymous>

```

<br>

**Javascript는 블록(Block)영역을 지원하지 않고 함수영역만 지원한다.**

```javascript
//전역
var variable = 'ECMAScript v5';
console.log('전역 variable: ', variable); // 'ECMAScript v5'

//블록영역
{
  var variable = 'Javascript';
  console.log('블록 영역 variable: ', variable); // 'Javascript''

  var array = [];
  console.log('블록 영역 array: ', array); // []
}

//함수 영역
function scope() {
  var variable = 'Function Scope';
  console.log('함수 영역 : ', variable);
}

scope(); // 'Function Scope'

//전역
console.log('전역 variable: ', variable);
// 블록영역을 지원한다면 'ECMAScript v5'출력이 되야하나 'Javascript' 가 출력
console.log('전역 array: ', array); // []

//결론: 블록영역을 지원하지 않아 전역이 오염됨
```

<br>

**ES6는 선택적으로 블록(Block)영역을 지원한다. (let, const) 키워드를 사용해 변수선언**

```javascript
//전역
var variable = 'ECMAScript v5';
console.log('전역 variable: ', variable); // 'ECMAScript v5'

//블록영역
{
  let variable = 'Javascript';
  console.log('블록 영역 variable: ', variable); // 'Javascript''

  const array = [];
  console.log('블록 영역 array: ', array); // []
}

//함수 영역
function scope() {
  let variable = 'Function Scope';
  console.log('함수 영역 : ', variable);
}

scope(); // 'Function Scope'

//전역
console.log('전역 variable: ', variable); //'ECMAScript'
console.log('전역 array: ', array); // 오류발생 array is not defiend

//결론: let, const 키워드를 사용하면 블록영역을 선택적으로 지원
```

## 함수실행 시점 & 스코프 체이닝

```Javascript
var fn_list = [];

// 반복문 (블록 영역)
for(var i = 0, l = 10; i < l; i++){
  fn_list.push(function(){
    // 함수 영역
    console.log(i)
  })
  console.log('반복문 내부 i :' , i);
}

console.log('반복문 외부 i : ', i);

//배열 데이터 순환 처리(콜백)
fn_list.forEach(function(f){
  f();
})




// 호이스트
var fn_list;
var i;
var l;

fn_list = [];

// 반복문 (블록 영역)
for(i = 0, l = 10; i < l; i++){
  fn_list.push(function(){
    // 함수 영역
    console.log(i)
  })
  console.log('반복문 내부 i :' , i);
}

console.log('반복문 외부 i : ', i);

//배열 데이터 순환 처리(콜백)
fn_list.forEach(function(f){
  f();
})
```

<br/>

## 클로저 & 함수실행 시점

```Javascript
var fn_list = [];

// 반복문 (블록 영역)
for(var i = 0, l = 10; i < l; i++){
  // 함수 호출 없이 즉시 실행
  // 클로저 생성
  fn_list.push(function(i){
    // 내부함수 반환
    return function(){
       console.log(i)
    }
  }(i));

}

console.log('반복문 외부 i : ', i);

//배열 데이터 순환 처리(콜백)
fn_list.forEach(function(f){
  f();
})
```

<br/>

## let & 블록영역 & 함수실행 시점

```Javascript
var fn_list = [];

// 반복문 (블록 영역)
for(let i = 0, l = 10; i < l; i++){
  // 함수 호출 없이 즉시 실행
  // 클로저 생성
  fn_list.push(function(){
       console.log(i)
    })
  };

//배열 데이터 순환 처리(콜백)
fn_list.forEach(function(f){
  f();
})
```

let 키워드는 블록영역을 지원!
<br/>

#### 호이스트

```javascript
// 함수 영역
function fire(condition) {
  // 조건문 (블록영역)
  if (condition) {
    var message = 'Fire Ball!';
    console.log(message);
  } else {
    console.log(message);
  }
}

fire(true); // 'Fire Ball!'
fire(false); // 에러가 아닌 undefiend

-------------------------------------------(
  // 함수 영역
  function fire(condition) {
    //호이스트 (영역의 최상단으로 변수 선언부가 끌어올려짐)
    var message;
    // 조건문 (블록영역)
    if (condition) {
      message = 'Fire Ball!';
      console.log(message);
    } else {
      console.log(message);
    }
  }
);

fire(true); // 'Fire Ball!'
fire(false); // 에러가 아닌 undefiend

-------------------------------------------(
  // let 키워드 사용

  // 함수 영역
  function fire(condition) {
    // 조건문 (블록영역)
    if (condition) {
      let message = 'Fire Ball!';
      console.log(message);
    } else {
      console.log(message);
    }
  }
);

fire(true); // 'Fire Ball!'
fire(false); // 에러 발생 Uncaught ReferenceError: message is not defined
```

<br />

**var** vs **let** vs **const**

**var**: 동일한 이름으로 중복 선언을 하더라도 문제가 발생하지 않는다.

```javascript
var tesla = 'TESLA';
var tesla = ['modelS', 'model3', 'modelX', 'modelY'];
```

<br>

**let** 동일한 이름으로 중복 선언시 오류가 발생된다.

데이터 값 변경이 필요할 경우 사용 권장

```javascript
let benz = 'BENZ';
let benz = ['A Class', 'C class', 'E class', 'S class'];
// Uncaught SyntaxError: Identifier 'benz' has already been declared
```

<br/>

**const** 초기 설정된 값을 다른 유형으로 변경할 경우 오류 발생 (상수 이기 때문)

but! 배열/객체유형일 경우 아이템 추가 및 변경 가능

데이터값 유형이 배열 / 객체일 때 사용 권장

```javascript
const bmw = 'BMW'
bmw = ['1Series', '3 Series', '5 Series', '7 Series'];
// Assignment to constant variable.
// let 과 마찬가지로 중복 선언도 불가능

const audi;
audi = 'AUDI';
// Uncaught SyntaxError: Missing initializer in const declaration
// 변수 선언과 할당이 동시에 이루어져야 한다!




```

**전역에 변수를 선언할 경우 var 변수는 window 객체에서 접근가능**

**let, const로 선언된 변수는 window 객체에서 접근 불가능**

#### IIFE -> BLOCK

자바스크립트는 함수영역을 지원하므로 전역오염 방지를 위해 IIFE 패턴을 사용하였으나 ES6의 let ,const 키워드 사용시 블럭영역을 지원하므로 IIFE 패턴을 사용안해도 된다.

```Javascript
//IIFE 패턴
(function (){
  var game = 'starcrft';
}());

console.log(game) // 참조 에러 발생


-------------------------------
{
   const game = 'starcrft';
}
console.log(game) // 참조 에러 발생
```

</details>

<!-- // 1주 2일차  -->

<!-- 1주 3일차 -->

<details>

<summary>3일차</summary>

### Javascript 클로져

<dl>
  <dt><h4>클로저 (closure)</h4><dt>
  <dd>Javascript의 매우 강력한 특성으로 독립적인 변수를 참조하는 함수를 말한다.  클로저에 정의된 함수는 그것이 작성된 환경을 <b>'기억'</b>한다.</dd>
<dl>

<p>중첩된 내부 함수는 상위 영역의 변수에 접근할수 있다.</p>

```javascript
// 일반적인 카운트 함수
function countUp() {
  var count = 1;
  console.log(count++);
}

for (var i = 1, l = 5; i < l; i++) {
  countUp();
}
// 기대와 달리 1 1 1 1 만 나옴
// 함수가 실행 될때마다 count 변수가 1로 선언 및 초기화 되기 때문

// 해결책
var count = 1;
function countUp() {
  console.log(count++);
}

for (var i = 1, l = 5; i < l; i++) {
  countUp();
}

// 의도한대로 1 2 3 4 결과가 나오나 전역을 오염시켜버림

//클로저 활용
function numberGenerator() {
  var count = 1;

  function countUp() {
    console.log(count++);
  }
  return countUp;
}

var number = numberGenerator();
for (var i = 1, l = 5; i < l; i++) {
  number();
}
```

```javascript
function makeAdder(x) {
  var y = 1;
  return function (z) {
    y = 100;
    console.log(x + y + z);
  };
}

var add5 = makeAdder(5);
var add10 = makeAdder(10);

//클로저에 x와 y의 환경이 저장됨

add5(30); // x=5 y=100 z=30  결과 값: 135
add5(50); // x=5 y=100 z=50  결과 값: 155

add10(30); // x=10 y=100 z=30   결과 값: 140
add10(50); // x=10 y=100 z=50   결과 값: 160
```

<br>

#### 실행컨텍스트

<p>자바스크립트는 단일 스레드 환경의 언어이므로 어느 시점이든 하나의 실행 컨텍스트를 실행한다.</p>
<p>브라우저는 스택(Stack)을 사용하여 실행 컨텍스트를 유지 관리하는데 스택은 LIFO(Last In First Out) 데이터 구조이다. 마지막에 푸시한 스택이 제일 먼저 꺼내진다. </p>

<br>

비슷해보이지만 서로 다른 실행컨텍스트를 가지고 있다.

</details>

<!-- // 1주 3일차 -->

<!-- 1주 4일차 -->

<details >

<summary>4일차</summary>

### Javascript 숫자

자바스크립트의 숫자는 모두 부동 소수점 형식이다.

#### 자바스크립트 수 의 상징적인 값

- +Infinity 양의 무한대
- -Infinit 음의 무한대
- NaN

#### 자바스크립트 숫자 값의 4가지 유형

- 10진수 (주로 사용)
- 2진수
- 8진수
- 16진수

#### 10진수

- 0으로 시작 가능
- 0으로 시작하는경우 다음에 오는 수가 8보다 작을경우 8진수로 해석할수 있다.
- 0으로 시작하는 수를 사용하지 않아야 한다.

#### 2진수

- 0 다음 b 또는 B 사용
- 0b 이후에 오는 숫자가 0 또는 1이 아니면 오류가 발생한다.

#### 8진수

- 0으로 시작
- 0 이후 숫자가 (0,1,2,3,4,5,6,7) 를 벗어나면 오류 발생

#### 16진수

- 0 다음 x 또는 X사용
- 0x 이후 숫자가 범위(0,1,2,3,4,5,6,7,8,9,a,b,c,d,e,f)를 벗어나면 오류발생

#### Javascript 부동 소수점의 계산오류

```javascript
0.1 + 0.2 = 0.30000000000000004
(0.1 _ 10) + (0.2 _ 10) / 10 = 0.3
(0.1 + 0.2).toPrecision(3) = 0.300
Number((0.1 + 0.2).toPrecision(3)) = 0.3


//헬퍼함수
function toPrecisionNumber(expression, precison){
  if(!expression || typeof expression !== 'number'){
    throw new Error('숫자 식을 전달해야 함');
  }
  precison = precison || 12;
  return Number(expression.toPrecision(precison))
}
```

자바스크립트의 계산정확도는 15자리까지 정확하다.

### Number 객체

#### 속성

Number 객체의 속성은 값이 정해져있는 상수이다.

```Javascript
Number.MAX_VALUE         // 1.7976931348623157e+308
Number.MIN_VALUE         // 5e-324
Number.NaN               // NaN (Not a Number)
Number.POSITIVE_INFINITY // Infinity
Number.NEGATIVE_INFINITY // -Infinity
Number.MIN_SAFE_INTEGER  // -9007199254740991
Number.MAX_SAFE_INTEGER  // 9007199254740991
```

#### 메서드

```Javascript
 //[스태틱 메서드 : IE 미지원]
 Number.parseFloat();
 Number.parseInt();
 Number.isFinite();
 Number.inInteger();
 Number.isNana();
 Number.isSafeInteger();

 parseFloat, parseInt은 같은경우
 window.parseFoloat(), widow.parseInt()를 주로 사용

 //[인스턴스 메서드]
 .toExponential()  // 지수표기법 (1000).toExponential() === '1e+3'  문자값 반환
 .toFixed()  // 소수점 절삭 (16.899999).toFixed(2) === '16.90' 문자값 반환
 .toPrecision() // (16.22222).toPrecision(3) === '16.2' 문자값 반환
```

자바스크립트는 숫자리터럴을 사용하면 그것을 숫자 객체로 사용하게끔 해준다.
<br>

### Math 객체

Math 는 생성자 함수가 아니기 떄문에 new Math 같은 생성자 사용안하고 Math 그자체로 이용

```Javascript
[속성]
Math.PI //  3.141592653589793

[메소드]
Math.min() // 최소값 Math.min(25, 75) === 25
Math.max() // 최대값 Math.max(35, 85) === 85
Math.random() // 난수 0 ~ 1사이의 실수
Math.floor() // 내림

var carOfTesla = ['modelS', 'model3', 'modelX', 'modelY'];
carOfTesla[Math.floor(Math.random() * carOfTesla.length)]

Math.round() // 반올림  Math.round(2.3) === 2  Math.round(2.5) === 3
Math.ceil()  // 올림 Math.ceil(2.3) === 3
Math.abs() // 절대값 Math.abs(-567.77) === 567.77
Math.pow() // 제곱 Math.pow(2, 10) === 1024
Math.sqrt() // 제곱근 Math.sqrt(64) === 8
Math.trunc() // 정수 반환(소수점 제거) Math.trunc(Math.PI) === 3
```

### String 객체

new String 생성자 함수를 통해 문자 객체생성

new 키워드 없이 String만 사용할 경우 문자화 시켜주는 함수기능

```javascript
String(20) === '20';
```

긴 리터럴 문자열 코드 가독성 높이면서 입력하기

```javascript
//  \ 활용
var html_code =
  '<header>\
                  <h1>\
                    <img src="assets/logo.svg" alt="웹사이트">\
                  </h1>\
                  <nav>\
                    <ul>\
                      <li><a href="">메뉴1</a></li>\
                      <li><a href="">메뉴2</a></li>\
                      <li><a href="">메뉴3</a></li>\
                      <li><a href="">메뉴4</a></li>\
                    </ul>\
                  </nav>\
                </header>\
                <main>\
                  <h2 class="a11yHidden">메인 컨텐츠</h2>\
                </main>';

// 배열 활용

var html_code = [
  '<header>',
  '<h1>',
  '<img src="assets/logo.svg" alt="웹사이트">',
  '</h1>',
  '<nav>',
  '<ul>',
  '<li><a href="">메뉴1</a></li>',
  '<li><a href="">메뉴2</a></li>',
  '<li><a href="">메뉴3</a></li>',
  '<li><a href="">메뉴4</a></li>',
  '</ul>',
  '</nav>',
  '</header>',
  '<main>',
  '<h2 class="a11yHidden">메인 컨텐츠</h2>',
  '</main>',
];

html_code.join('');
```

#### 문자접근

```javascript
'kimMinCheol'.charAt(6); // C
'kimMinCheol'[6]; // C
```

#### 문자열 비교

```javascript
'apache' < 'beta'; // true
'가' < '나'; // true

'가'.localeCompare('나'); // -1
'나'.localeCompare('가'); // 1
'나'.localeCompare('나'); // 0
```

자바스크립트는 문자'값'을 문자'객체'처럼 사용가능하게 해준다. (문자 객체의 기능들 사용가능)
하지만 둘은 다르다

```javascript
var game = 'game';
var newGame = new String('game');

game === newGame; //false
```

특정문자열내에 문자값이 있는지 확인하는 방법
(예시: 브라우저 체크)

```javascript
//일치하는 문자의 인덱스값 반ㄴ환
window.navigator.userAgent; //
('Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Safari/537.36');
window.navigator.userAgent.indexOf('Chrome'); // 87

window.navigator.userAgent.indexOf('Chrome') > -1;
//일치하는 문자열이 없을경우 -1반환
```

문자값 자르기

```javascript
// string.slice(state, [end]);
// start 부터 end-1 까지 문자를 복사하여 반환
// end 인자값이 없을경우 나머지 전체를 복사하여 반환

var kmc = 'KimMinCheol';
var kmcSliceStart = kmc.slice(3); // 'MinCheol'
var kmcSliceStartEnd = kmc.slice(3, 7); // 'MinC'

console.log(kmc); // 'KimMinCheol' 그대로 유지
```

대, 소문자 변환

```javascript
var kmc = 'kimmincheol';
var upperKmc = kmc.toUpperCase(); // 'KIMMINCHEOL'
var lowerKmc = upperKmc.toLowerCase(); // 'kimmincheol'
```

문자열 일부 변환

```javascript
var kmc = 'KimMinCheol';
var kms = kmc.replace('Cheol', 'Sic'); // 'KimMinSic'
```

</details>

<!-- 1주 5일차 -->

<details open>

<summary>5일차</summary>

#### ES6 Teamplate Literal

ES6의 템플릿 리터럴을 사용하면 문자열 이스케이프를 사용 안해도 된다.

```javascript
// ES5
var comment = 'ES6 이전은 \'이스케이프\' "따옴표" 를 사용해야 합니다';

// ES6
let comment = `ES6 이후로는 이스케이프 '따옴표'를 사용 안해도 됩니다.`;
```

문자열 접합

```javascript
// ES5
var company = {
  brand: '테슬라',
  fuel: '전기',
  product: '자동차',
};

console.log(
  company.brand + '는 ' + company.fuel + company.product + ' 생산을 하는 회사다'
);

// ES6
const company = {
  brand: '테슬라',
  fuel: '전기',
  product: '자동차',
};

console.log(
  `${company.brand}는 ${company.fuel}${company.product} 생산을 하는 회사다`
);
```

ES6의 템플릿 리터럴은 띄어씌기, 개행 자바스크립트 표현식을 지원한다.
ES6로 진행되는 작업에는 적극 활용!

### 확장된 문자 객체

#### .includes()

```javascript
//ES5 문자열 표함 여부 확인방법
window.navigator.userAgent.indexOf('Chrome') > -1;
//일치하는 문자열이 있을경우  true 반환

//ES6의 .includes() 사용
window.navigator.userAgent.includes('Chrome');
//일치하는 문자열이 있을을경우  true 반환
```

#### .startsWith() 어떤 문자열이 특정문자로 시작하는지 확인하여 Boolean 값으로 반환

```javascript
//ES5
var car = '제네시스 그랜져 소나타 아반떼';
car.substr(0, 4) === '제네시스'; //true
car.substr(5, 1) === '그'; //true

//헬퍼 함수
function startsWidth(word, find, start) {
  start = start || 0;
  return word.substr(start, find.length) === find;
}
startsWidth(car, '그랜져', 5); //trues

//ES6는 startsWith 자체적으로 지원
var car = '제네시스 그랜져 소나타 아반떼';
car.startsWith('그랜져', 5); //true
```

#### .endsWith() 어떤 문자열이 특정문자로 끝나는지 확인하여 Boolean 값으로 반환

```javascript
//ES5
var car = '제네시스 그랜져 소나타 아반떼';
var index = car.length - 3;
car.substr(index, 3) === '아반떼'; //true

//헬퍼 함수
function endsWith(word, find, end) {
  end = (end || word.length) - find.length;
  //전체 글자수에서 찾고자 하는 문자의 글자수를 뺀다.
  var lastIndex = word.indexOf(find, end);
  return lastIndex !== -1 && lastIndex === end;
}

endsWith(car, '아반떼'); //true
endsWith(car, '소나타', 12); // true

//ES6는 endsWith 자체적으로 지원
var car = '제네시스 그랜져 소나타 아반떼';
car.endsWith('아반떼'); //true
car.endsWith('소나타', 12); //true
```

#### .repeat() 특정문자열을 특정개수만큼 반복한 후 새로운 문자열 반환

```javascript
//ES5 문자열 반복
var repeatWord = '반복할 문자열 ';

function repeat(word, count) {
  //카운트 초기화
  count = count || 0; // 입력한 count가 있을 경우 count, 없을 경우 0
  if (count === 0) {
    return '';
  }
  var combine = word; // 반복할 문자열 복사
  while (--count) {
    combine += word;
  }
  return combine;
}

repeat(repeatWord, 5); // "반복할 문자열 반복할 문자열 반복할 문자열 반복할 문자열 반복할 문자열 "

//ES6 문자열 반복
const repetaWord = 'ES6 오오오 ';
repetaWord.repeat(5); //"ES6 오오오 ES6 오오오 ES6 오오오 ES6 오오오 ES6 오오오 "
```

</details>

<!-- // 1주 5일차 -->
