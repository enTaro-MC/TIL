# TIL

UI Interaction Senior, 2020 TIL (Today I Learned)

## 1주차

<details open><summary>1일차</summary>

<div>
<div>
### 값 복사 vs 값 참조

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

</div>

<div>

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
함수 영역내에 해당 키워드가 없을 경우 상위 영역에서 변수를 찾고 상위 영역에도 없을 경우 전역변수를 생성하거나 변수명이 있을경우 해당변수에 값을 참조시켜버린다. (전역오염)`

**지역 :**부분의 프로그래밍 언어의 경우 함수 내에서 유효한 지역변수를 제공

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

</div>

<div>

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

</div>

<div>

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

</div>

</div>

</details>
