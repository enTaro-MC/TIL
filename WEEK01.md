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

<details open>

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
