# TIL

UI Interaction Senior, 2020 TIL (Today I Learned)

## 2주차

<details>

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

<details>

<summary>7일차</summary>

### [ES6] Arrow Function

function 키워드를 대신해서 간소화 한것이 화살표 함수이다.

```javascript
var sum = function (x, y) {
  return x + y;
};

const sum1 = (x, y) => {
  return x + y;
};

const sum2 = (x, y) => x + y;
//중괄호를 생략하면 retrun도 생략 가능하다.

const squared = n => n * n;
//매개변수가 1개일 경우 매개변수 괄호도 생략 가능하다.
```

```javascript
// 객체의 메서드에 화살표 함수를 사용할 경우
// this는 자신의 객체를 가리키는게 아니라 객체의 상위요소를 가르키게 된다.
// 메서드 내부에서의 화살표 함수를 쓸 경우 this는 객체를 가르킨다!

const videoGameConsole = {
  _name: 'PlayStation',
  _games: [],
  printGames: function () {
    console.log('this', this);
    this._games.forEach(game => {
      console.log('this', this);
    });
  },
};

videoGameConsole._games.push('갓오브워', '라스트오브어스', 'GTA');
videoGameConsole.printGames();

// this = videoGameConsole 객체

//메서드에 화살표 함수를 쓸 경우
const videoGameConsole = {
  _name: 'PlayStation',
  _games: [],
  printGames: () => {
    console.log(this); //window 객체
    this._games.forEach(game => {
      console.log(this); //this가 window 객체를 가리켜 forEach 참조 에러
    }, this);
  },
};

videoGameConsole._games.push('갓오브워', '라스트오브어스', 'GTA');
videoGameConsole.printGames();
```

결론:

- ES6를 사용하여 프로젝트를 진행한다면 함수표현식에서는 화살표 함수 적극 활용!
- 객체의 속성(메서드)로는 사용하지 말자!
- 객체 속성(메서드) 내부에서는 적극 활용하자

### Default Parameters

ES6는 함수의 매개변수의 기본값을 설정 할수 있다

```javascript
//ES5
function calcPayment(price, tax, discount) {
  if (!price) {
    throw new Error('price 매개변수 값은 필수입니다.');
  }
  tax = tax || 0.1;
  discount = discount || 0;
  return Math.floor(price * (1 + tax) - (1 - discount));
}

calcPayment(10000, 0.3, 0.1);

//ES6

function isRequired(param) {
  throw new Error(`${param} 매개 변수값은 필수 입니다.`);
}
function calcPayment(price = isRequired('price'), tax = 0.1, discount = 0) {
  return Math.floor(price * (1 + tax) - (1 - discount));
}

calcPayment();

// ES6 비구조 할당
function calcPayment({ price = isRequired('price'), tax = 0.1, discount = 0 }) {
  return Math.floor(price * (1 + tax) - (1 - discount));
}

calcPayment({ price: 30000 });
```

### Rest Parameters (나머지 매개변수)

함수의 나머지 매개변수를 하나로 모아 배열값으로 사용 가능하다.

```javascript
function sum(r = 0, ...nums) {
  nums.forEach(n => (r += n));
  return r;
}

console.log(sum(0, 1, 3, 10)); //0을 제외한 나머지 매개변수
```

### Spread Operator (전개 연산자)

```javascript
// ES5

// 배열복제
var oddNumber = [1, 3, 5, 7];
for (var copyOddNumber = [], i = 0, l = oddNumber.length; i < l; i++) {
  copyOddNumber[i] = oddNumber[i];
}
console.log(copyOddNumber); // [1, 3, 5, 7];

var copyOddNumberMap = oddNumber.map(function (number) {
  return number;
});
console.log(copyOddNumberMap); // [1, 3, 5, 7];

var copyOddNumberSlice = oddNumber.slice();
console.log(copyOddNumberSlice); // [1, 3, 5, 7];

//ES6
let oddNumber = [1, 3, 5, 7];
let copyOddNumberSpreadOperator = [...oddNumber]; // [1, 3, 5, 7]
```

```javascript
// 배열 결합
let oddNumber = [1, 3, 5, 7];
let evenNumber = [2, 4, 6, 8];

let numbers = [...oddNumber, ...evenNumber];
console.log(numbers); //[1,3,5,7,2,4,6,8]
```

</details>

<details>

<summary>8일차</summary>

### 배열 객체

배열 객체는 모든것을 수집할수 있다. 배열은 '값'의 집합이다.

```javascript
// 배열의 생성
var array = [];

// index 로 배열 아이템에 접근
var numArray = [1, 2, 3, 4, 5];
console.log(numArray[2]); // 3

//배열 객체 아이템 순환 처리 .forEach
numArray.forEach(function (number, i) {
  console.log(number); // 1, 2, 3, 4, 5
});

//배열 객체에 새로운 아이템 추가 (Last In) .push()
numArray.push(8);
console.log(numArray); // [1, 2, 3, 4, 5, 8]

// 배열 객체의 마지막 아이템 제거 (Last Out) .pop()
numArray.pop();

// 배열 객체에 새로운 아이템 추가 (First In) .unshift()
numArray.unshift();

// 배열 객체에 새로운 아이템 추가 (First Out) .shift()
numArray.shift();

// 배열 객체 아이템 인덱스 찾기
var indexArray = [10, 20, 30];

indexArray.indexOf(10); // 0
indexArray.indexOf(20); // 1
indexArray.indexOf(30); // 2
indexArray.indexOf(5); // -1

// 배열 값이 존재하면 index 값 반환
// 배열 값이 존재하지 않는다면 -1 반환

// 배열 객체 아이템 제거
var codeEditor = [
  {
    id: 1,
    name: 'VS Code',
    developer: 'Microsoft',
  },
  {
    id: 2,
    name: 'Atom',
    developer: 'Github',
  },
  {
    id: 3,
    name: 'Bracket',
    developer: 'Adobe',
  },
  {
    id: 4,
    name: 'Webstorm',
    developer: 'Jetbrain',
  },
];
var choiceEditor = codeEditor.splice(0, 2); // 0번째 인덱스부터 2개의 배열 아이템값

// 배열 객체 모든 아이템 제거 .splice() 배열 원본 수정
codeEditor.length = 0;
codeEditor = [];

// 배열 복사
var copyEditor = codeEditor.slice();

// 배열 검증
Array.isArray(codeEditor); //true

// 배열 순서 정렬 .sort()  배열 원본 수정
[-1, 20, 1, 9].sort(); // -1, 1, 20, 9

[-1, 20, 1, 9].sort(function (a, b) {
  return a - b; // 오름차순
});
[-1, 20, 1, 9].sort(function (a, b) {
  return b - a; // 내림차순
});

// 배열 내 객체 순서 정렬 .sort() 컴페어 함수 필요
codeEditor.sort(function (e1, e2) {
  return e1.developer < e2.developer ? -1 : e1.developer > e2.developer ? 1 : 0;
});

// 배열 순서 뒤집기 .reverse()   (배열 원본 수정)
codeEditor.reverse();

// 배열 내 모든 요소 각각에 주어진 함수를 호출하여 결과값 반환 .map();
codeEditor.map(function (editor) {
  editor.value = 'IDE';
  return editor;
});
```

```javascript
// 배열의 속성
array.length;

// 메서드

// 원본 배열 데이터 수정
array.push();
array.pop();
array.unshift();
array.reverse();
array.sort();
array.splice();

// 원본 배열 데이터 보존
array.concat();
array.indexOf();
array.lastIndexOf();
array.join();
arrray.slice();
array.toString();

// 반복 메서드 (원본 베열 데이터 보존)
array.forEach(function (item, index) {});
array.map(function (item, index) {});
array.filter(function (item, index) {});
array.every(function (item, index) {});
array.some(function (item, index) {});
array.reduce(function (item, index) {});
```

### ES6 Array Addition

#### Static Methods

```javascript
// 유사 배열을 배열 객체로 변환하여 배열의 속성 활용

//ES5
var htmlChildren = document.querySelectorAll('html > *');
console.log(htmlChildren); // [head, body] NodeList (유사배열)

var arrayHtmlChildren = Array.prototype.slice.call(htmlChildren);

//배열 객체의 call 메서드를 활용하여 배열로 복사

Array.isArray(arrayHtmlChildren); // true

//ES6  Array.from
let htmlChildren = document.querySelectorAll('html > *');
let arrayHtmlChildren = Array.from(htmlChildren);

Array.isArray(arrayHtmlChildren); // true


Array.from(arrrayLike, mapFunc?, thisArg?);


// Array.of  배열생성
const data = Array.of(3, 6, 9);  // [3, 6, 9]
```

#### Array.prototype 메서드

```javascript
// Array.prototype

//ES5
var oddNumbers = [1, 3, 5, 7, 9];
for (var i = 0, l = oddNumbers.length; i < l; i++) {
  console.log(oddNumbers[i]); // 1, 3, 5, 7, 9
}

oddNumbers.forEach(function (oddNumber, index) {
  console.log(oddNumber); // 1, 3, 5, 7, 9
});

oddNumbers.map(function (oddNumber, index) {
  return oddNumber; // [1, 3, 5, 7, 9] 배열반환
});

//ES6 keys values entries

let evenNumbers = [2, 4, 6, 8, 10];

for (let index of evenNumbers.keys()) {
  console.log(index); // 0, 1, 2, 3, 4
}

for (let value of evenNumbers.values()) {
  console.log(value); // 2, 4, 6, 8, 10
}

// 조건에 부합하는 배열아이템 찾기 (첫번째 매치되는 아이템만 반환)

// ES5
var numbers = [100, 105, 103, 109];

function findItemArray(array, callback) {
  for (var i = 0, l = array.length; i < l; i++) {
    if (callback(array[i], i, array)) {
      return array[i];
    }
  }
}

var item = findItemArray(numbers, function (item, index, array) {
  return item > 100 && item < 105;
});

console.log(item); // 103

//ES6 .find()
let numbers = [100, 105, 103, 109];
let item = numbers.find(item => (item > 100) & (item < 105));
console.log(item); // 103

// 조건에 맞는 배열 인덱스 찾기 (첫번째 매치되는 아이템만 반환)

// ES5
var numbers = [100, 105, 103, 109];

function findItemIndexArray(array, callback) {
  for (var i = 0, l = array.length; i < l; i++) {
    if (callback(array[i], i, array)) {
      return i;
    }
    return -1;
  }
}

var item = findItemIndexArray(numbers, function (item, index, array) {
  return item > 105;
});

console.log(item); // 3

// ES6 .findIndex()
var item = numbers.findIndex(item => item > 105);
console.log(item); // 3

// 배열 내에 찾는 값이 있다면 해당 index 값 반환

//ES5 indexOf()
// 단순 === 비교
// NaN === NaN // false;
[false, 10, NaN, {}, ''].indexOf(''); //4
[false, 10, NaN, {}, ''].indexOf(false); // 0
[false, 10, NaN, {}, ''].indexOf(NaN); // -1
[false, 10, NaN, {}, ''].indexOf({}); // -1
[false, 10, NaN, {}, '', []].indexOf({}); // -1

// ES6 findIndex()
// Object.is() 비교
//
[false, 10, NaN, {}, '']
  .findIndex(x => Object.is(x, NaN)) // 2
  [(false, 10, NaN, {}, '')].findIndex(x => Object.is(x, {})) // -1
  [(false, 10, NaN, {}, '')].findIndex(x => Array.is(x, [])); //-1

// 배열에 아이템이 포함되어 있는지 확인

//ES5
var numbers = [100, 105, 103, 109];

function includeItemArray(array, item) {
  return array.indexOf(item) > -1;
}
numbers.indexOf(100) > -1; // true
numbers.indexOf(50) > -1; // false

//ES6  .includes()
let numbers = [100, 105, 103, 109];
numbers.includes(100); // true

// fill() 배열아이템을 교체
//ES6  .fill()
let numbers = [100, 105, 103, 109];
numbers.fill({}); // [{}, {}, {}, {}]

let numbers = [100, 105, 103, 109];
numbers.fill({}, 1, numbers.length - 1); // [1, {}, {}, 109]

// copyWithin
let numbers = [100, 105, 103, 109];

number.copyWithin(1, 2); // [100, 103, 109, 109]
number.copyWithin(-3, -2); // [100, 103, 109, 109]
```

</details>

<details open>

<summary>9일차</summary>

## 배열 객체 메서드 MDN 문서 살펴보기

### forEach

- forEach() 메서드는 주어진 함수를 배열 요소 각각에 실행한다.
- **undefined** 를 반환한다.
- 예외를 던지지 않고서는 forEach()를 중간에 멈출수 없다.
- forEach()는 배열을 변형하지 않는다. 하지만 callback 함수가 변형 시킬 수 있다.

```javascript
// Array.prototype.forEach()
//

const array1 = ['a', 'b', 'c'];
array1.forEach(element => console.log(element));

// a
// b
// c
```

```javascript
// 구문
arr.forEach(callback(currentvalue[, index[, array]])[, thisArg])
```

**매개 변수**

  <ul>
    <li>callback : 각 요소에 대해 실행할 함수 (다음 세가지의 매개변수를 받음)
      <ul>
        <li>currentValue: 처리할 현재 요소</li>
        <li>index: 처리할 현재 요소의 인덱스 (옵션)</li>
        <li>array: forEach()를 호출한 배열 (옵션)</li>
      </ul>
    </li>
    <li>thisArg: callback을 실행할 때 this로 사용할 값</li>
  </ul>

```javascript
let words = ['one', 'two', 'three', 'four'];
words.forEach(word => {
  console.log(word);
  // 전체 배열에서 맨 앞 아이템이 빠져 나머지 배열들의 순서가 땡겨져 three 출력이 생략되어버린다.
  if (word === 'two') {
    words.shift();
  }
});

// one
// two
// four

console.log(words); // ['two', 'three', 'four']
```

### map

- map() 메서드는 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 **새로운 배열을 반환** 한다.
- map()은 배열을 변형하지 않는다. 하지만 callback 함수가 변형 시킬 수 있다.

```javascript
const array1 = [1, 4, 9, 16];
const map1 = array1.map(x => x * 2);
console.log(map1);
// [2, 8, 18, 32]
```

```javascript
// 구문
arr.map(callback(currentValue[, index[, array]])[, thisArg])
```

**매개 변수**

  <ul>
    <li>callback : 각 요소에 대해 실행할 함수 (다음 세가지의 매개변수를 받음)
      <ul>
        <li>currentValue: 처리할 현재 요소</li>
        <li>index: 처리할 현재 요소의 인덱스 (옵션)</li>
        <li>array: forEach()를 호출한 배열 (옵션)</li>
      </ul>
    </li>
    <li>thisArg: callback을 실행할 때 this로 사용할 값</li>
  </ul>

### forEach()와 map()의 차이

- forEach()의 경우 배열 요소마다 한번씩 콜백함수를 실행, undefiend 값 반환
- map()의 경우 배열 요소마다 한번씩 콜백함수를 실행한 결과 값들을 모아 새로운 배열을 반환

### filter()

- 주어진 콜백함수의 테스트를 통과하는 모든 요소를 모아 새로운 배열로 반환

```javascript
const words = [
  'spray',
  'limit',
  'elite',
  'exuberant',
  'destruction',
  'present',
];

const result = words.filter(word => word.length > 6);

console.log(result);
// ["exuberant", "destruction", "present"]
```

```javascript
const fruits = ['apple', 'banana', 'grapes', 'mango', 'orange'];

/**
 * 검색 조건에 따른 배열 필터링(쿼리)
 */
const filterItems = query => {
  return fruits.filter(
    el => el.toLowerCase().indexOf(query.toLowerCase()) > -1
  );

  // 배열 내 모든 아이템을 소문자화 시키고, 매개변수로 입력한 값을 소문자한 시킨값이 포함된 아이템 반환
};

console.log(filterItems('ap')); // ['apple', 'grapes']
console.log(filterItems('an')); // ['banana', 'mango', 'orange']
```

### slice()

- 어떤 배열의 begin부터 end까지(end 미포함)에 대한 얕은 복사본을 새로운 배열 객체로 반환
- 원본 배열은 바뀌지 않는다.

```javascript
const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];

console.log(animals.slice(2));
// ["camel", "duck", "elephant"]

console.log(animals.slice(2, 4));
// ["camel", "duck"]

console.log(animals.slice(1, 5));
// ["bison", "camel", "duck", "elephant"]
```

```javascript
// 구문
arr.slice([begin[, end]])
```

**매개 변수**

  <ul>
    <li>begin: 추출을 시작할 인덱스 값 </li>
    <li>end: 추출을 종료할 인덱스 값 (옵션)</li>
  </ul>

### splice()

- 배열의 기존 요소를 삭제 또는 교체, 새 요소 추가
- 배열원본의 내용을 변경
- 제거한 요소를 담은 배열 반환, 아무 요소도 제거 안했을 경우 빈 배열 반환

```javascript
const months = ['Jan', 'March', 'April', 'June'];
months.splice(1, 0, 'Feb');
console.log(months);
// ["Jan", "Feb", "March", "April", "June"]

months.splice(4, 1, 'May');

console.log(months);
// ["Jan", "Feb", "March", "April", "May"]
```

```javascript
// 구문
array.splice(start[, deleteCount[, item1[, item2[, ...]]]])
```

**매개 변수**

  <ul>
    <li>start: 배열변경을 시작할 인덱스 값, 음수일 경우 배열의 끝에서부터 요소를 세어나감 </li>
    <li>deleteCount: 배열에서 제거할 요소의 수, 0 이하 일 경우 어떤 요소도 삭제 안함 (옵션) </li>
    <li>item1, item2: 배열에 추가 할 요소, 없을 경우 splice()는 요소를 제거하기만 함 (옵션)</li>
  </ul>

```javascript
// 요소 제거 없이 2번 인덱스에 요소 추가
var myFish = ['angel', 'clown', 'mandarin', 'sturgeon'];
var removed = myFish.splice(2, 0, 'drum');

console.log(myFish); // ["angel", "clown", "drum", "mandarin", "sturgeon"]
console.log(removed); // []

// 3번 인덱스에서 한개 요소 제거
var myFish = ['angel', 'clown', 'drum', 'mandarin', 'sturgeon'];
var removed = myFish.splice(3, 1);
console.log(myFish); // ["angel", "clown", "drum", "sturgeon"]
console.log(removed); // ["mandarin"]

// 2번 인덱스에서 두개 요소 제거
var myFish = ['parrot', 'anemone', 'blue', 'trumpet', 'sturgeon'];
var removed = myFish.splice(myFish.length - 3, 2);

console.log(myFish); // ["parrot", "anemone", "sturgeon"]
console.log(removed); // ["blue", "trumpet"]

//2번 인덱스를 포함한 이후 모든 요소 제거
var myFish = ['angel', 'clown', 'mandarin', 'sturgeon'];
var removed = myFish.splice(2);

console.log(myFish); // ["angel", "clown"]
console.log(removed); // ["mandarin", "sturgeon"]
```

### sort()

- 배열 요소를 정렬 후 그 배열을 반환
- 복사본이 아닌 배열 원본을 정렬함

```javascript
const months = ['March', 'Jan', 'Feb', 'Dec'];
months.sort();
console.log(months);
// expected output: Array ["Dec", "Feb", "Jan", "March"]

const array1 = [1, 30, 4, 21, 100000];
array1.sort();
console.log(array1);
// expected output: Array [1, 100000, 21, 30, 4]
```

```javascript
// 구문
arr.sort([compareFunction]);
```

**매개 변수**

  <ul>
    <li>compareFunction: 정렬 순서를 정의하는 함수, 생략할 경우 각 요소이 문자열 변환 유니코드 포인트 값에 따라 정렬 (옵션)</li>
  </ul>

```javascript
//  숫자 배열 오름차순 정렬
var numbers = [4, 2, 5, 1, 3];
numbers.sort(function (a, b) {
  return a - b;
});
console.log(numbers); //[1, 2, 3, 4, 5]

// 숫자 배열 내림차순 정렬
//  숫자 배열 오름차순 정렬
var numbers = [4, 2, 5, 1, 3];
numbers.sort(function (a, b) {
  return b - a;
});
console.log(numbers); // [5, 4, 3, 2, 1]
```

```javascript
// 객체 정렬
var items = [
  { name: 'Edward', value: 21 },
  { name: 'Sharpe', value: 37 },
  { name: 'And', value: 45 },
  { name: 'The', value: -12 },
  { name: 'Magnetic', value: 13 },
  { name: 'Zeros', value: 37 },
];

// value 기준으로 정렬
items.sort(function (a, b) {
  return a.value > b.value ? 1 : a.value < b.value ? -1 : 0;
});

// name 기준으로 정렬
items.sort(function (a, b) {
  var nameA = a.name.toUpperCase(); // ignore upper and lowercase
  var nameB = b.name.toUpperCase(); // ignore upper and lowercase
  return nameA > nameB ? 1 : nameA < nameB ? -1 : 0;
});
```

### reduce()

- 배열의 각 요소에 대하진 리듀서 함수를 실행
- 누적 계산의 결과 값을 반환

```javascript
const array1 = [1, 2, 3, 4];
const reducer = (accumulator, currentValue) => accumulator + currentValue;

// 1 + 2 + 3 + 4
console.log(array1.reduce(reducer));
//  10

// 5 + 1 + 2 + 3 + 4
console.log(array1.reduce(reducer, 5));
// expected output: 15
```

**매개 변수**

  <ul>
    <li>callback(accumulator,currentValue )
      <ul>
        <li>accumulator: 콜백의 반환값을 누적한다. 첫번째 호출이면서 초기값을 제공한 경우 초기값이 된다.</li>
        <li>currentValue: 현재값</li>
        <li>currentIndex: 현재 요소의 인덱스, initialValue 값이 있을 경우 0, 없을경우 1부터 시작한다. (옵션)</li>
        <li>array (옵션)</li>
      </ul> 
    </li>
    <li>initialValue: callback의 최초 호출에서 제공하는 초기값, 빈 배열에서 초기값 없이 reduce()를 호출하면 오류가 발생한다. (옵션)</li>
  </ul>

```javascript
// 배열의 모든 값 합산
var sum = [3, 6, 9, 12].reduce((accumulator, currentValue) => (accumulator + currentValue;));
console.log(sum); // 30



// 객체 배열에서의 값 합산
var initialValue = 0;
var sum = [{x: 1}, {x:2}, {x:4}].reduce(function (accumulator, currentValue) {
    return accumulator + currentValue.x;
}, initialValue)

console.log(sum) // 6


var initialValue = 0;
var sum = [{x: 1}, {x:2}, {x:4}].reduce(function (accumulator, currentValue) {
    return accumulator + currentValue.x;
}, 1)  //initialValue가 없을 경우 [object Ojbect]24 값 반환

console.log(sum) // 6

```

### .isArray()

- .isArray() 메서드는 인자가 배열인지 판별하는 메서드이다.
- 인자가 Array 객체라면 true, 아니면 false 를 반환한다.

```javascript
Array.isArray([]); // true
Array.isArray([1, 3, 5, 7]); //true
Array.isArray(new Array()); // true
Array.isArray(new Array('a', 'b', 'c', 'd')); // true
Array.isArray(Array.prototype); // true  배열 프로로타입 자체도 배열이다.
```

### .some()

- 배열안의 어떤 요소라도 주어진 판별 함수를 통과하는지 테스트 한다.
- 빈배열을 넣을 경우 false 반환
- 요소중 하나라도 true일 경우 true 반환
- 호출한 배열을 변형시키지 않는다.

```javascript
[12, 5, 8, 1, 4].some(elem => elem > 10); // true
[2, 5, 8, 1, 4].some(elem => elem > 10); // false
```

### .every()

- 배열안의 모든 요소가 주어진 판별 함수를 통과하는지 테스트 한다.
- 모든 요소가 참일 경우 true 반환
- 거짓인 요소를 발견할 경우 즉시 false 반환
- 호출한 배열을 변형시키지 않는다.

```javascript
[12, 15, 18, 11, 14].every(elem => elem > 10); // true
[2, 5, 8, 1, 4].every(elem => elem > 10); // false
```

</details>
