# TIL

UI Interaction Senior, 2020 TIL (Today I Learned)

## 3주차

<!-- 3주 1일차  -->

<details>

<summary>1일차</summary>

### 객체 / 상속

Javascript의 모든 객체들은 Object들의 자손이다.

```js

// 객체생성방법
var obj = {}
var ojb1 = new Object{}

// 객체 속성 추가
obj.name = 'object';
console.log(obj) // {name: "object"}
console.log(obj.name); // 'obejct'

// 객체 속성 제거
delete obj.name
console.log(obj) // {}
console.log(obj.name) // undefined
```

```js
var hm_son7 = {
  birth: '1992년 7월 8일',
  name: '손홍민',
  nationality: '대한민국',
  hometown: '춘천',
  nickname: ['소니', '손세이셔널'],
  hobby: ['독서', '게임'],
  height: '182cm',
  weight: '77kg',
  job: '프로 축구선수',
  position: '윙어',
  club: '토트넘 핫스퍼(Tottenham Hotspur) F.C.',
  picture: 'https://goo.gl/GK11yQ',
  sns: {
    facebook: 'https://www.facebook.com/heungminsonofficial',
    instagram: 'https://www.instagram.com/hm_son7',
  },
};

// 객체가 속성을 가지고 있는지 확인하는 법  in
'nickname' in hm_son7; // true
'sns' in hm_son7; // true

// 객체 속성 순환 for ~in  객체 속성(열거) enumerable
for (var prop in hm_son7) {
  console.log(prop, ':', hm_son7[prop]);
}
/*
birth : 1992년 7월 8일
name : 손홍민
nationality : 대한민국
hometown : 춘천
nickname : (2) ["소니", "손세이셔널"]
hobby : (2) ["독서", "게임"]
height : 182cm
 weight : 77kg
job : 프로 축구선수
position : 윙어
club : 토트넘 핫스퍼(Tottenham Hotspur) F.C.
picture : https://goo.gl/GK11yQ
sns : {facebook: "https://www.facebook.com/heungminsonofficial", instagram: "https://www.instagram.com/hm_son7"}
*/
```

```js
// 객체 병합(Mixins 패턴)

var mixin = function () {
  // 빈 객체 생성
  var mixin_obj = {};
  for (var i = 0, l = arguments.length; i < l; ++i) {
    var o = arguments[i]; // 매개변수로 들어온 객체들을 임시객체 o 에 참조
    // 임시객체 o 순환
    for (var key in o) {
      var value = o[key];

      // hasOwnProperty() 객체가 특정 프로퍼티를 갖고 있는지 boolean 값을 반환한다.
      if (o.hasOwnProperty(key)) {
        mixin_obj[key] = value;
      }
    }
  }
  return mixin_obj;
};

var car = {
  type: 'normal',
  wheels: 4,
  handle: 1,
  mirrors: { side: 2, back: 1 },
  engine: '3000cc',
  weight: '313kg',
  booster: false,
};

var extend_car_features = {
  type: 'super',
  wheels: 6,
  booster: true,
  engine: '4497cc',
  weight: '452kg',
  maximum: {
    power: '2248 horse-power, 21800 rpm',
    torque: '194kgm, 17100 rpm',
    speed: '695km + alpha',
  },
};

var super_car = mixin(car, extend_car_features);
```

### Object Static Method

```js
//객체 능력 상속 Object.create()

// o 객체는 car 객체의 능력을 상속
var o = Object.create(car);

console.log(o.wheels); // 4
console.log(car.wheels); // 4

o.wheels = 8; // 속성값을 덮어씌움
console.log(o.wheels); // 8
console.log(car.wheels); // 4

// 객체 속성 정의  Object.definedProperty(obj, property, descripter)

// 객체에 직접 속성을 정의하거나 이미 존재하는 객체의 속성을 수정한 뒤 그 객체를 반환한다.
// 객체 속성을 수정, 열거, 삭제등을 못하게 옵션을 줄 수 있다.

/*
descripter: {

  // 데이터 기술
  writable: false, // 값 변경 가능 여부 결정
  enumerable: false // 속성 열거 가능 여부
  configurable: false // 속성 제거 가능 여부
  value: undefined // 객체 속성 값 설정 
}
*/

Object.defineProperty(car, 'name', { value: 'benz' });

car.name = 'bmw'; // 'benz'  writable:false 이기때문
delete car.name; // false   configurable: false 이기 때문

for (var prop in car) {
  console.log(prop, ':', car[prop]);
}

// undefiend enumerable: false라 name 속성 열거 불가능

Object.defineProperty(car, 'fuel', {
  value: 'oil',
  configurable: true,
  writable: true,
  enumerable: true,
});

car.fuel = 'electric'; // electric
delete car.fuel; // true

for (var prop in car) {
  console.log(prop, ':', car[prop]);
}

// fuel: electric
```

```js
/*
descripter: {

  // 데이터 접근 기술

  // 속성 값을 얻는 목적의 getter용 함수, 함수의 반환값이 속성값이 된다.
  get: undefiend  


  // 속성 값을 설정하는 목정의 setter용 함수, 단 하나의 인자만 받으며 속성 값으로 할당된다.
  set: undefiend
 
 ※ value 또는 writable과 동시에 get 또는 set 키를 함께 가지고 있으면 오류가 발생
}
*/
var visible = true;
Object.defineProperty(car, 'display', {
  get: function () {
    console.log('getter');
    return visible;
  },
  set: function (value) {
    console.log('setter');
    visible = value;
  },
});

// 여러개의 속성 추가
Object.defineProperties(o, {
  margin: {
    value: '외부 여백',
  },
  padding: {
    get: function () {
      return pad;
    },
    set: function (value) {
      pad = value;
    },
  },
});
```

#### 객체 확장 차단

```js
// 객체의 속성울 추가하지 못하게 함 (삭제 가능)
Object.preventExtensions(car);

car.production = 'hyundai';
car.production; // undefined

delete car.wheels; // true
// 객체의 속성이 추가가 가능한지 확인
Object.isExtensible(car); // false
```

#### 객체 밀봉

```js
// 객체가 밀봉되어있는지 확인
Object.isSealed(car); // false

// 객체를 밀봉하면 속성 추가 및 삭제가 불가능하다.
// 쓰기 가능한 속성 값의 경우 밀봉후에도 변경 가능하다
Object.seal(car);

car.fuel = 'electric'; // undefiend   속성 추가 불가
delete car.weight; // false  속성 삭제 불가
car.handle = '2'; // 변경가능한 속성은 변경 가능
```

#### 객체 동결

```js
// 객체의 속성을 삭제 및 변경 불가능
// 밀봉 +  속성 값 변경 차단
Object.freeze(car);

car.type = 'hyper car'; // 'normal'
delete car.type; // false

// 객체 동결 확인
Object.isFrozne(car); // true
```

### Instance Method

```javascript
// .hasOwnProperty() 자기 자신의 속성인지 확인

var co = Object.create(hm_son7);

// 자기 자신의 속성이 아니라 hm_son7 객체로부터 상속받은 객체이므로 false 출력
co.hasOwnProperty('birth'); // false

co.ad = 'Super Corn';

// 자기 자신의 속성이므로 true 출력
co.hasOwnProperty('ad'); // true
```

</details>

<!-- // 3주 1일차  -->

<!-- 3주 2일차  -->

<details open>

<summary>2일차</summary>

### 생성자 함수 / 프로토타입

- Javascript 객체(Instance)는 생성자(Constructor) 함수를 통해 생성된다
- new 연산자 뒤에 생성자 함수를 시랭하면, 내장 객체 or 사용자 정의 객체 인스턴스를 생성한다.

```js
var slide = new Constructor();
```

- 생성자 함수는 관례적으로 첫글자를 대문자로 작성하여 일반함수와 구분한다.
- 자바스크립트의 모든 함수객체는 정의와 동시에 함수의 Prototype 객체를 참조하는 속성 .prototype을 가지게 된다.

```js
function Carousel(selector) {}
// Carousel.prototype 속성에 참조된 객체는 .constructor 속성을 가지며 이 속성을 통해 생성자 함수 Carousel을 참조한다.
```

```js
var list = new Array();
list.constructor; // Array()

Array.prototype.constructor === Array; // true
list.constructor === Array.prototype.constructor; // true
```

```js
function Carousel(selector) {
  this.el = document.querySelector(selector);
}

var main_carousel = new Carousel('body');

// instanceof 연산자: 생성자의 prototype 속성이 객체의 프로토타입 체인 어딘가에 있는지 판별하여 boolean 값 반환

main_carousel instanceof Carousel; // true

// main_carousel 이 Carousel의 인스턴스이다

//ES6에서는 class 문법을 지원한다.
class BreadCrumbs {}
new BreadCrumbs();
```

<code>new Carousel()</code> 이 실행될때 발생되는 상황

1. Carousel.prototype을 상속하는 새로운 객체가 생성된다.
2. 새로 생성된 객체에 연결된 this와 함께 생성자 함수 Carousel이 호출된다.

   - 함수에서의 this는 함수를 실행시킨 주체를 가르키나 new 연산자를 통해 실행된 생성자 함수의 this는 생성된 객체를 가르킨다.
   - new Carousel은 new Carousel()과 동일하다.
   - 전달인자가 없을 경우 전달인자 없이 Carousel이 호출된다.

3. 생성자 함수를 실행하면 생성된 객체가 반환된다. (return 기본값)
   그러나 명시적으로 사용자가 다른 객체를 반환할 경우, 반환결과물을 덮어 쓸 수 있다.

```js
var carousel1 = new Carousel();
var carousel2 = new Carousel();
var carousel3 = new Carousel('.main_slide');

// carousel1, carousel2, carousel3은 Carousel 생성자 함수를 통해 생성된 객체이다.
// carousel1, carousel2, carousel3은  _proto_ 속성을 가지고 있다.
// Carousel.prototype 객체의 속성 및 메서드와 링크(상속)되어있다.

carousel1.constructor === Carousel; // true
carousel1 instanceof Carousel; // true

function Carousel(selector) {
  this.el = document.querySelector(selector);

  // return this (기본값)
  return window; // 결과값을 덮어 씌울수 있다.
}
```

prototype 에 접근하는 방법

```js
function Carousel(selector) {
  this.el = document.querySelector(selector);
}

var main_carousel = new Carousel('body');

main_carousel.constructor.prototype;
main_carousel.__proto__; //ie 11부터 지원

Object.getPrototypeOf(main_carousel); // Object 내장메서드 .getPrototypeOf() 이용

Object.getPrototypeOf(main_carousel) === Carousel.prototype;
```

생성된 이후에 객체 속성 추가 방법

```js
// carousel1, carousel2, carousel3의 공통 속성은  Carousel 생성자 함수를 통해 참조된 Carousel.prototype 객체의 것이다.

// 생성된 이후에 추가된 속성은 각자만의 속성이다.
carousel1.id = 1;
carousel2.mode = 'Vertical';
carousel3.animation = 'fade';
```

```js
// Carousel 생성자 함수를 정의한후 인자로 el. options 를 받을수 있다.
// 전달된 인자는 생성된 객체(this)의 el, options 속성에 할당된다.
function Carousel(el, options) {
  this.el = el;
  options ? (this.options = options) : null;

  return this; // 기본값
}
// Carousel.prototype 객체에 추가된 속성(메서드)는 Carousel 생성자를 통해 생성된 모든 인스턴스의 공통 속성(메서드) 이다.
Carousel.prototype.play = function () {
  console.log('슬라이드 실행');
};
Carousel.prototype.pause = function () {
  console.log('슬라이드 일시 정지');
};
Carousel.prototype.prevSlide = function () {
  console.log('이전 슬라이드로 이동');
};
Carousel.prototype.nextSlide = function () {
  console.log('다음 슬라이드로 이동');
};

var mainCarousel = new Carousel(document.body, {
  autoplay: true,
  duration: 3000,
});

mainCarousel.play(); // '슬라이드 실행'
mainCarousel.pause(); // '슬라이드 일시 정지'
mainCarousel.prevSlide(); // 이전 슬라이드로 이동
mainCarousel.nextSlide(); // 다음 슬라이드로 이동

var bannerCarousel = new Carousel(document); //옵션은 안넣음

bannerCarousel.play(); // '슬라이드 실행'
bannerCarousel.pause(); // '슬라이드 일시 정지'
bannerCarousel.prevSlide(); // 이전 슬라이드로 이동
bannerCarousel.nextSlide(); // 다음 슬라이드로 이동

// 개별속성은 인자로 받아 구분하게 하고
// 공통속성에 해당하는것은 prototype 객체에 맡겨두는게 성능면에서 좋다.
```

### Strict Mode

new 연산자 없이 생성자 함수를 실행시키게 되면 일반 함수로 실행이 되어 제대로 작동안하고 전역을 오염시키고 오류도 발생시키지 않아 디버깅이 힘들어지는 경우가 생긴다.

```js
function phone(numbers) {
  console.log('phone :: this', this);
  numbers = numbers.replace(/-/g, '');
  return numbers;
}

function Phone(numbers) {
  console.log('phone :: this', this);
  this.numbers = numbers;
  this.addNumbers = numbers;
}

phone('010-5519-1689'); // this = 함수를 실행시킨 주체
Phone('010-5519-1689'); // this = 함수를 실행시킨 주체
//전역에 없던 addNumbers 변수 생성

new Phone('010-5519-1689'); // this -> Phone

// 이런 경우를 방지하기 위해 오류를 발생시키게 해야한다.
```

```js
(function () {
  name = '김민철';
})();
//스코프체이닝을 하면서 변수를 찾다 없을 경우 상위영역에 변수를 생성시켜버린다 (전역오염)

//  'use strict' 키워드를 활용한 엄격한 모드
(function () {
  'use strict';
  age = '34';
})();

// Uncaught ReferenceError: age is not defined  참조오류 발생
```

### 객체 지향 프로그래밍 (Object Oriented Programming)

```js
// 생성자 함수 설정 (개별적으로 가질수 있는 속성은 인자로 받아온다.)
function Animal(type) {
  this.type = type;
}

// 공통속성
Animal.prototype.brain = true;
Animal.prototype.legs = 0;

// 사람
var human = new Animal('인간');
human.brain; // true
human.legs; // 0

// 애완동물 생성자
function Pet(kind) {
  // 생성자 함수 Animal 상속
  Animal.call(this, '애완동물');
  this.kind = kind;
}

Pet.prototype = Object.create(Animal.prototype);
Pet.prototype.legs = 4;
Pet.prototype.fleas = 0;
Pet.prototype.constructor = Pet; // 덮어씌우기

var cat = new Pet('고양이');
cat.constructor === Pet; // true

//상속 헬퍼 함수
function inherit(subClass, superClass) {
  subClass.prototype = Object.create(superClass.prototype);
  subClass.prototype.constructor = subClass;
}

function Cat(name, age, color) {
  Pet.call(this, '고양이');
  this.name = name;
  this.age = age;
  this.color = color;
}

Cat.prototype.fleas = 4;
inherit(Cat, Pet);
var sebaschan = new Cat('세바스찬', 1, '하얀색');
sebaschan.legs;
```

### 객체 지향 프로그래밍 용어

<dl>
  <dt>Class</dt>
  <dd>객체 속성(Properties)을 정의한다. (설계도면)</dd>
  <dt>Object</dt>
  <dd>Class의 인스턴스(객체) (설계 도면을 통해 구현된 제품) </dd>
  <dt>Property</dt>
  <dd>객체의 속성 (age, name 등과 같은 명사형태)</dd>
  <dt>Method</dt>
  <dd>객체의 기능 (on(), off() 같은 동사형태)</dd>
  <dt>Constructor</dt>
  <dd>인스턴스 생성 순간에 호출 실행되는 메서드</dd>
  <dt>Inheritance </dt>
  <dd>Class는 다른 Class로부터 상속을 받을수 있다.</dd>
  <dt>Encapsulation</dt>
  <dd>Class는 해당 객체의 속성, 메서드만 정의 할 수 있다.(외부 접근불가)</dd>
  <dt>Abstraction</dt>
  <dd>복잡한 상속, 메서드, 객체 속성의 결합은 반드시 현실 모델을 시뮬레이션할 수 있어야 한다.</dd>
  <dt>Polymorphism</dt>
  <dd>다른 Class 들이 같은 메서드나 속성으로 정의될 수 있다.</dd>
</dl>

</details>

<!-- // 3주 2일차  -->

<!-- 3주 3일차  -->
<details>

<summary>3일차</summary>

### [ES6] Shorthand Properties

ES6는 객체의 속성과 값의 이름이 같다면 키값만 입력하는 속기형 작성법을 지원한다.

```js

var games = ['스타크래프트', '리그 오브 레전드', '오버워치', '워크래프트'];
var animations = ['진격의 거인', '원피스', '강철의 연금술사', '나루토'];

var movies = [
  {
    title: '기생충',
    director: '봉준호',
  },
  {
    title: '인셉션',
    director: '크리스토퍼 놀란',
  },
];
// ES5
var favorites = {
  games : games
  animations: animations,
  movies: movies,
};

// ES6
let favorites = {
  games,
  animations,
  movies,
};
```

### Object Enhancements

```js
// 향상된 객체 표기법
(function (global) {
  'use strict';

  let name = 'SM7',
    maker = 'RenaultSamsung',
    boost = 'powerUp';

  //private 감춰진 속성 외부에서 접근 불가
  let _wheel = 4;

  const car = {
    go() {}, // 메서드의 경우 go: function(){}  방식에서 변경
    // 계산된 속성
    ['stop']() {},
    [boost]() {},

    // getter
    get wheel() {
      return this._wheel;
    },

    set wheel(new_wheel) {
      this._wheel = new_wheel;
    },
  };

  const newbee = {
    name,
    maker,
    __proto__car: car, // car의 능력을 상속받는다
    [`${name.replace(7, 5)}${maker}${
      boost.slice(0, 1).toUpperCase() + boost.slice(0, 1)
    }`]() {},
  };

  global.car = car;
})(window);
```

### Symbol

고유하고 수정 불가능한 데이터 타입이며 주로 객체 속성들의 식별자로 사용된다.

```js
((global = window) => {
  let _wheel = Symbol('wheel');

  global.car = {
    // 등록된 심볼을 속성으로 사용
    [_wheel]: 4,
    get wheel() {
      return this[_wheel];
    },
    set wheel(new_wheel) {
      if (typeof new_wheel !== 'number') {
        throw new Error('전달 인자 유형은 숫자여야 합니다.');
      }
      this[_wheel] = new_wheel > 4 ? new_wheel : 4;
    },
  };
})();
```

### 구조 분해 할당

```js
// 객체 구조 분해 할당

// ES5
var game = {
  title: '디아블로',
  webPage: 'https://diablo4.blizzard.com/ko-kr/',
  developer: 'blizzard',
};

var title = game.title;
var webPage = game.webPage;
var developer = game.developer;

// ES6
let game = {
  title: '디아블로',
  webPage: 'https://diablo4.blizzard.com/ko-kr/',
  developer: 'blizzard',
};

let { title, webPage, developer } = game;

//IIFI 패턴  ()
((global = window, { title, webPage, developer }) => {
  console.log(title);
  console.log(webPage);
  console.log(developer);
})(null, game);

// 필요한 속성만 가져올수도 있다.
((global = window, { title }) => {
  console.log(title);
  // console.log(webPage);
  // console.log(developer);
})(null, game);

((
  global = window,
  { title: gameTitle, webPage: promotionPage, developer: developCompany } // 특정한 변수로 지정해줄수 있다.
) => {
  console.log(gameTitle);
  console.log(promotionPage);
  console.log(developCompany);
})(null, game);
```

```js
// 배열 구조 분해

const people = [
  {
    id: 1,
    name: '이정도',
    username: 'jd1386',
    email: 'lee.jungdo@gmail.com',
    phone: '010-3192-2910',
  },
  {
    id: 2,
    name: '김재완',
    username: 'lastrites2018',
    email: 'jaewan@gmail.com',
    phone: '02-879-5000',
  },
  {
    id: 3,
    name: '김성은',
    username: 'sunnysid3up',
    email: 'sunny@daum.net',
    phone: '010-4280-1991',
  },
  {
    id: 4,
    name: '이주연',
    username: 'yyijoo',
    email: 'jooyeon@gmail.com',
    phone: '010-2940-1401',
  },
];

// ES5
people.forEach(function (person) {
  var name = person.name;
  var username = person.username;
  var email = person.email;
  var phone = person.phone;

  console.log(name, username, email, phone);
});

// ES6

people.forEach(({ name, username, email, phone }) => {
  console.log(name, username, email, phone);
});

let [, , 김성은, 이주연] = people; // 순서 중요!
function logEmail({ email }) {
  console.log(email);
}

logEmail(김성은);
logEmail(이주연);
```

</details>
<!-- // 3주 3일차  -->

<!-- 3주 4일차  -->
<details >

<summary>4일차</summary>

### [ES6] Class

ES6에서는 기존의 프로토타입 기반의 객체지향 프로그래밍 대신, 클래스기반의 객체지향 프로그래밍 방법을 사용할 수 있다.

```js
((global = window) => {
  // ES5
  //생성자 함수
  function Dom(el) {
    this.el = el;
  }

  // 스태틱 메서드
  Dom.createEls = function () {};

  Dom.prototype.init = function () {};
  Dom.prototype.bind = function () {};

  //ES6
  class Dom {
    // 생성자(클래스를 통해 객체를 생성할때 실행)
    constructor(el) {
      this.el = el;
    }

    // 스태틱 메서드 (클래스 메서드)
    static createELs() {}

    // 인스턴스 메서드
    // prototype.init()
    init() {}
    bind() {}
  }
})();
```

- Class 는 함수가 아니므로 호이스트 되지 않는다.
- Class는 식으로 사용 가능하다
- Class는 내부에 변수를 작성 할수 없다.
- Class는 콤마를 사용하지 않는다.

```js
// 클래스 선언 이전에 사용하면 참조 오류가 발생한다.

new Game();
class Game {}

//Uncaught ReferenceError: Game is not defined 참조 오류 발생

const Game = class {};
```

#### Private Data

자바스크립트에서는 비공개 데이터를 공식적으로 지원하지 않는다. 관례적으로 \_ 언더바를 붙여 비공개 데이터라고 알린다.

```js
(() => {
  class Coffee {
    constructor(bean, type) {
      // 공개 데이터
      this.bean = bean;

      // 비공개 데이터 (실제로 데이터가 보호되어 있지는 않고 있다.)
      this._type = type;
    }
  }
})();

// Object.assign()  활용
(() => {
  class Coffee {
    constructor(bean, type) {
      // 비공개 데이터
      // 완전한 데이터 비공개 관리가 가능하나, 메모리 누수가 발생한다.
      // 설정도 불가능함
      Object.assign(this, {
        getBean() {
          return bean;
        },
        getType() {
          return type;
        },
      });
    }
  }
})();

// 심볼 + 게터 /세터 활용 (주의: Class를 여러번 사용하면 _bean의 데이터가 덮어씌여짐.)
(() => {
  let _bean = Symbol('bean');
  class Coffee {
    constructor(bean) {
      // 비공개 데이터
      this[_bean] = bean;
    }

    get pea() {
      return this[_bean];
    }
    set pea(new_bean) {
      this[_bean] = new_bean;
    }
  }
})();

// WeakMap 활용
(() => {
  let _bean = new WeakMap();

  class Coffee {
    constructor(bean) {
      _bean.set(this, bean);
    }

    get pea() {
      return _bean.get(this);
    }
    set pea(new_pea) {
      _bean.set(this, new_pea);
    }
  }
})();
```

#### Class 기반 상속

```js
class Coffee {
  constructor(bean) {
    this.bean = bean;
  }
  parch(time) {
    console.log(`${time}만큼 ${this.bean}을 볶다`);
  }
}


//extends 키워드 사용

class Latte extend Coffee {
  constructor(bean, milk){
    super(bean);  // 상속받는 클래스의 생성자를 사용한다면 super 메서드 필수!
    this.milk = milk;
  }

  // 메서드 오버라이드
  parch(hour){
    super.parch(hour/2);
    console.log(`${hour/4}만큼 ${this.milk}를 넣고 끓인다`);
  }
}

Object.getPrototypeOf(Latte) === Coffee;
Latee.__proto__ === Coffee;


// Object.setPorototypeOf()  클래스가 아닌 객체 상속

const coffee = {
  bean: 'arabica',
  parch(time){console.log(`${time}만큼 ${this.bean}을 볶다`);}
}
const latte = {
  milk: 'shake',
  blend(source){console.log(`${source}와 ${this.milk} ${this.bean}을 혼합한다.`)}
}

Object.setPrototypeOf(latte, coffee);

```

</details>
<!-- // 3주 4일차  -->

<!-- 3주 5일차  -->
<details open>

<summary>5일차</summary>

### [ES6] Map

- Map 객체는 속성, 값 쌍으로 구성된 객체이다.
- 객체는 문자, Symobol만 키(속성)로 사용 할수 있는 반면, 맵 객체는 어떤값도 키로 사용 가능하다.
- 객체는 객체에 저장된 데이터 개수를 알수 없지만 맵은 .size 속성을 이용하면 알수 있다.
- 데이터 콜렉션을 다룰때 사용하면 좋다.

```js
// 맵 객체 생성 (iterable)
let capitals = new Map();

// 아이템(키, 값) 추가
capitals.set('한국', '서울');
capitals.set('중국', '북경');
capitals.set('미국', '워싱턴 D.C');

console.log(capitals); // {"한국" => "서울", "중국" => "북경", "미국" => "워싱턴 D.C"}

// 저장된 아이템 갯수 출력
capitals.size; // 3

// 저장된 아이템 출력
capitals.get('한국');
capitals.get('일본');

// 저장된 키 값 확인
if (!capitals.has('일본')) {
  capitals.set('일본', '동경');
}

// 요소 삭제
capitals.delete('일본'); // true
capitals.delete('러시아'); //  없는데이터를 제거 할때 는 false

// 모든 요소 삭제
capitals.clear();
```

### [ES6] WeakMap

```js
// 데이터 (객체)
let arr = [1, 3, 5, 7],
  obj = { key: 'value' };

// WeakMap 객체 생성
let wmap = new WeakMap();

// 아이템 추가
wmap.set(arr, 'array').set(obj, 'object');

// 아이템 사이즈
wmap.size; // undefined

// 객체가 아닌 데이터를 키값으로 추가 할 수 없다.
wmap.set(true, 'yes'); // Uncaught TypeError: Invalid value used as weak map key at WeakMap.set

// 아이템 소유여부 확인
wmap.has(obj); // true

// 아이템제거
wmap.delete(arr); // true

// 세트 순환
// WeakMap은 열거가 불가능하므로 순환이 불가능하다.

//사용 법

// 비공개 데이터 관리
(() => {
  let_ = new WeakMap();
  class OffCanvasMenu {
    constructor(el, options) {
      _.set(this, { el, options });
    }

    toggle() {
      let $ = _.get(this);
      $.el.classList.toggle('is-active');
    }
  }
})();
```

</details>

<!-- // 3주 5일차  -->
