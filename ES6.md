# Vue.js를 위한 ES6

-   ECMAScript2015와 동일한 용어
-   2015년은 ES5(2009년)이래로 진행한 첫 메이저 업데이트가 승인된 해
-   최산 Front-End Framework인 React, Angular, Vue에서 권고하는 방식
-   ES5에 비해 문법이 간결해져서 익숙해지면 코딩을 훨씬 편하게 할 수 있음
-   Babel
    -   구 버전 브라우저 중에서는 ES6의 기능을 지원하지 않는 브라우저가 있으므로 transpiling이 필요
    -   ES6의 문법을 각 브라우저의 호환 가능한 ES5로 변환하는 컴파일러

## ES6의 특징

### const & let - 새로운 변수 선언 방식

-   블록 단위 `{ }`로 변수의 범위가 제한되었음
-   `const`: 한 번 선언한 값에 대해서 변경할 수 없음(상수 개념)
-   `let`: 한 번 선언한 값에 대해서 다시 선언할 수 없음, 값을 재할당할 수 있음

### ES5 vs ES6

#### ES5의 특징 - 변수의 Scope

-   기존 자바스크립트(ES5)는 `{ }` 에 상관없이 스코프가 설정된다.

```js
var sum = 0;
for (var i = 1; i <= 5; i++) {
    sum = sum + i;
}
console.log(sum); // 15
console.log(i); // 6
```

#### ES5의 특징 - Hoisting

-   Hoisting이란 선언한 함수와 변수를 해석기가 가장 상단에 있는 것처럼 인식한다.
-   js해석기는 코드의 라인 순서와 관계없이 함수선언식과 변수를 위한 메모리 공간을 먼저 확보한다.(함수표현식은 해당하지 않음)

```js
// function statement(함수선언식)
function sum() {
    return 10 + 20;
}
// function expression(함수표현식)
var sum = function () {
    return 10 + 20;
};
```

-   따라서, `function a()`와 `var`는 코드 최상단으로 끌어올려진 것(hoisted)처럼 보인다.

```js
function willBeOverriden() {
    return 10;
}
willBeOverriden(); // 5
function willBeOverriden() {
    return 5;
}
```

아래와 같은 코드를 실행할 때 자바스크립트 해석기가 어떻게 코드 순서를 재조정할까요?

```js
var sum = 5;
sum = sum + i;
function sum() {
    return true;
}
var i = 10;
```

`i`라는 변수가 나중에 초기화되었지만, 호이스팅으로 인해 오류가 발생하지 않는다.

```js
// #1 - 함수 선언식과 변수 선언을 hoisting
var sum;
function sum() {
    return true;
}
var i;

// #2 - 변수 대입 및 할당
sum = 5;
sum = sum + i;
i = 10;
```

#### ES6 - `{ }` 단위로 변수의 범위가 제한됨

```js
let sum = 0;
for (let i = 1; i <= 5; i++) {
    sum = sum + i;
}
console.log(sum); // 10
console.log(i); // Uncaught ReferenceError: i is not defined
```

#### ES6 - `const`로 지정한 값 불가능

```js
const a = 10;
a = 20; // Uncaught TypeError: Assignment to constant variable
```

하지만, 객체나 배열의 내부는 변경할 수 있다.

```js
const a = {};
a.num = 10;
console.log(a); // { num: 10 }

const a = [];
a.push(20);
console.log(a); // [20]
```

#### ES6 - `let` 선언한 값에 대해서 다시 선언 불가능

```js
let a = 10;
let a = 20; // Uncaught SyntaxError: redeclaration of let a
```

#### ES6 - const, let

```js
function f() {
    {
        let x;
        {
            // 새로운 블록 안에 새로운 x의 스코프가 생김
            const x = 'sneaky';
            x = 'foo'; // 위에 이미 const로 x를 선언했으므로 다시 값을 대입하면 에러 발생
        }
        // 이전 블록 범위로 돌아왔기 때문에 `let x`에 해당하는 메모리 값을 대입
        x = 'bar';
        let x = 'inner'; // Uncaught SyntaxError: Identifier 'x' has already been declared
    }
}
```

### Arrow Function - 화살표 함수

-   함수를 정의할 때 `function`이라는 키워드를 사용하지 않고 `=>`로 대체
-   흔히 사용하는 콜백 함수의 문법을 간결화
-   TODO: Arrow Function 의 Scope

```js
// ES5 함수 정의 방식
var sum = function (a, b) {
    return a + b;
};

// ES6 함수 정의 방식
var sum = (a, b) => {
    return a + b;
};

sum(10, 20);
```

-   화살표 함수 사용 예시

```js
// ES5
var arr = ['a', 'b', 'c'];
arr.forEach(function (value) {
    console.log(value); // a, b, c
});

// ES6
var arr = ['a', 'b', 'c'];
arr.forEach((value) => console.log(value)); // a, b, c
```

### Enhanced Object Literals - 향상된 객체 리터럴

-   객체의 속성을 메서드로 사용할때 `function`예약어를 생략하고 생성 가능

```js
var dictionary = {
    words: 100,
    // ES5
    lookup: function () {
        console.log('find words');
    },
    // ES6
    lookup() {
        console.log('find words');
    },
};
```

-   객체의 속성명과 값이 동일할 때 아래와 같이 축약 가능

```js
var figuers = 10;
var dictionary = {
    // figures: figures
    figures,
};
```

### Modules - 자바스크립트 모듈화 방법

-   자바스크립트 모듈 로더 라이브러리(AMD, CommonJS)기능을 js 언어 자체에서 지원
-   호출되기 전까지는 코드 실행과 동작을 하지 않는 특징이 있음

```js
// libs/math.js
export function sum(x, y) {
    return x + y;
}
export var pi = 3.141593;

// main.js
import { sum } from 'libs/math.js';
sum(1, 2);
```

-   Vue.js에서 마주칠 `default` export

```js
// util.js
export default function (x) {
    return console.log(x);
}

// main.js
import util from 'util.js';
console.log(util); // function(x) { return console.log(x); }
util('hi');

// app.js
import log from 'util.js';
console.log(log);
log('hi');
```
