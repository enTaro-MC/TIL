# TIL

UI Interaction Senior, 2020 TIL (Today I Learned)

## 2주차

<details open>

<summary>6일차</summary>

### 함수

#### 함수 정의 및 호출, 실행

```javascript
// function
function fnName() {
  return '함수 정의';
}

//이름이 없는 함수 (표현식)
var fnName2 = function () {
  return '함수 정의2';
};

// 함수 호출 및 실행  실행연산자 : ()
fnName();
fnName2();
```

#### 함수 결과 반환 or 종료 (return)

```javascript
function includeString(str) {
  if (!str || typeof str !== 'string') {
    return;
  }
  return number;
}
```

함수는 기본적으로 undefined를 반환한다.

#### 함수 재귀

```javascript
function loop(n) {
  if (n > 15) {
    return;
  }
  loop(++n);
}
```

#### 함수 스택 (Last In First Out)

```javascript
function foo(i) {
  if (i < 0) {
    return;
  }
  console.log('begin:' + i);
  foo(i - 1); //재귀함수

  console.log('end :' + i); //재귀함수로 인한 일시정지
}
foo(3);
```

#### 다중중첩함수 (스코프체이닝은 성능 이슈를 만들수도 있으므로 주의를 요한다.)

#### this는 함수의 실행 주체이다.

#### 함수는 객체의 소유가 가능하다.

#### 전달인자 객체(arguments)

```javascript
function sum() {
  for (var total = 0, i = 0, l = arguments.length; i < l; i++) {
    total += arguments[i];
  }
  return total;
}

//arguments 전달인자 객체 (유사배열)

sum(12, 15, 18, 21, 24);

// 유사배열은 index, length 를 가지고 있어 배열처럼 사용가능하나 pop, push, shift, unshift등의 배열 메서드는 사용이 불가능하다.
```

#### 즉시실행함수 (IIFE)

```javascript
(function () {
  //코드 보호 (전역오염 방지)
})()(function (global, document) {
  //IIFE에 인자전달 -> 내부 매개변수 참조
  //스코프체이닝에 의한 성능문제 해결방법
})(window, window.document);
```

#### 함수 객체

속성과 메서드는 객체가 가질수 있는것 <br>
함수는 객체이다

###### 함수객체의 속성

<ul>
  <li>function.name (함수 자신의 이름)</li>  
  <li>function.length (전달인자 갯수) (arguments.length 를 더 많이 쓴다.)</li>
</ul>

###### 함수객체의 메서드

```javascript
function thisIsFunction(x, y, z) {
  console.log('this ->', this);
  console.log('x ->', x);
  console.log('y ->', y);
  console.log('z ->', z);
}

// .call()
thisIsFunction.call(document.head, 10, 20, 30);
// 함수를 실행시킬때 첫번째 인자로 this를 설정해줄수 있음

// .apply()
thisIsFunction.apply(doucment.body, [10, 20, 30]);
// 함수를 실행시킬때 첫번째 인자로 this를 설정해줄수 있음, 배열을 사용해서 인자 전달

// .bind()
thisIsFunction.bind(document.head, 10, 20, 30);
// 함수를 바로 실행시키지 않고 첫번째 인자로 this를 설정해 줄 수 있음
// 필요한 시점에 실행시킬수 있음 (이벤트에서 활용!)
```

</details>
