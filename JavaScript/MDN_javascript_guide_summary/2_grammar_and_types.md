# 2. 문법과 타입

## 기본

- 대소문자를 구별하며, Unicode 문자셋을 이용
- 명령을 문(statement)라고 부르며, 세미콜론(;)으로 분리
- 스페이스, 탭, 줄바꿈 문자를 공백이라 함
- javascript의 스크립트 소스는 왼쪽에서 오른쪽으로 탐색되어 토큰, 제어문자, 줄바꿈 문자, 주석이나 공백인 입력 요소의 열(sequence)로 바뀜
- 부작용 방지를 위해 항상 세미콜론을 추가해 문을 끝내는 것을 권장

## 주석

```javascript
// 한 줄 주석

/* 이건 더 긴,
 여러 줄 주석 입니다.
/*
/* 그러나, /* 중첩된 주석은 쓸 수 없습니다 */ SyntaxError */
```

## 선언

javascript에서는 총 3가지의 선언이 있음

```javascript
var
```

변수를 선언. 추가로 동시에 값을 초기화.

```javascript
let
```

블록 범위(scope) 지역 변수를 선언. 추가로 동시에 값을 초기화

```javascript
const
```

읽기 전용 상수를 선언.

### 변수

어플리케이션에서 값에 상징적인 이름으로 변수를 사용합니다. 변수명은 식별자(identifier)라고 불리며 특정 규칙을 따릅니다.

식별자는 문자, 밑줄(_) 혹은 달러 기호($)로 시작해야하는 반면 이후는 숫자(0-9)일 수도 있습니다. 대소문자를 구분합니다.

ex. Number_hits, temp99, $credit, _name 등

### 변수선언

변수 선언은 아래 3가지 방법으로 가능합니다.

- `var` 키워드. 예를 들어 `var x = 42`. 이 구문은 지역 및 전역 변수를 선언하는데 사용
- `x = 42` 로 선언하는 경우, 전역 변수로 지정됨. strict mode[^1]의 javascipt에서는 경고를 발생시켜 실행이 안됨
- `let` 키워드. 예를 들어 `let y = 13`. 이 구문은 블록 범위 지역 변수를 선언하는데 사용

[^1]: `use strict `를 통해 선언, 오류 검사 추가. [링크](https://msdn.microsoft.com/ko-kr/library/br230269(v=vs.94).aspx)

### 변수평가

지정된 초기값 없이 `var` 또는 `let`문을 사용해서 선언된 변수는 `undefined` 값을 가짐

선언되지 않은 변수에 접근을 시도하는 경우 `ReferenceError` 예외가 발생

```javascript
var a;
console.log(a;) // undefined 로 로그 남음
console.log(b;) // ReferenceError 예외 던짐
```

`undefined`를 사용하여 변수값이 있는지 확인할 수 있음

```javascript
var input;
if(input === undefined) {
    console.log("input은 할당되지 않음");
} else {
    console.log("input은 할당됨");
}
```

`undefined` 값은 `boolean` 문맥에서 사용될 때 `false`로 동작.

```javascript
var myArray = [];
if (!myArray[0]) {
    console.log("undefined는 false로 동작");
}
```

`undefined`값은 수치 문맥에서 사용될 때 NaN[^2]으로 변환

```javascript
var a;
a + 2 // NaN으로 평가
```

[^2]: Not-A-Number, 숫자가 아님. [링크](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/NaN)

`null` 값을 평가할때, 수치 문맥에서는 0, `boolean` 문맥에서는 `false`로 동작.

```javascript
var n = null;
console.log(n * 32); // 콘솔에 0으로 로그가 남음
```

### 변수범위

- 전역변수: 어떤 함수 바깥에 변수를 선언, 현재 문서의 다른 코드에서 해당 변수를 사용 가능
- 지역변수: 어떤 함수 내부에 변수를 선언, 오직 그 함수 내에서만 사용할 수 있음

ES6 이전의 javascript는 block문 범위가 없고, 블록 내에 선언된 변수는 그 블록 내에 존재하는 함수(혹은 전역 범위)에 지역적.

```javascript
if (true) {
    var x = 5;
}
console.log(x); // 5
```

ES6에 도입된 `let` 선언을 사용하는 경우 동작이 바뀜

```javascript
if (true) {
    let y = 5;
}
console.log(y); // ReferenceError: y is not defined
```

### 변수호이스팅

예외를 받지 않고 나중에 선언된 변수를 참조할수 있으나, 이때 끌어올려진 변수는 `undefined`값을 반환한다. 심지어 이 변수를 사용 혹은 참조한 후에 선언 및 초기화 하더라도 여전히 `undefined` 값을 반환 한다. 이 개념을 호이스팅(hoisting)이라 하며, 코드를 명확하게 만들어 주기 위해서는 함수내의 모든 `var`문은 가능한 함수 상단 근처에 두는 것이 좋다.

```javascript
/**
 * Example 1
 */
console.log(x === undefined); // logs "true"
var x = 3;


/**
 * Example 2
 */
// undefined 값을 반환함.
var myvar = "my value";

(function() {
  console.log(myvar); // undefined
  var myvar = "local value";
})();
```

위 코드는 아래 코드와 동일하다.

```javascript
/**
 * Example 1
 */
var x;
console.log(x === undefined); // logs "true"
x = 3;

/**
 * Example 2
 */
var myvar = "my value";

(function() {
  var myvar;
  console.log(myvar); // undefined
  myvar = "local value";
})();
```

### 전역변수

전역변수는 사실 global 객체의 속성(property)이다. 웹페이지에서 global 객체는 `window`이므로, `window.variable` 구문을 통해 전역변수를 설정하고 접근할 수 있다.

그 결과, window 혹은 frame의 이름을 지정하면, 한 window 혹은 frame에 선언된 전역변수에 접근할 수 있다. 예를 들어, `phoneNumber`라는 변수가 문서에 선언된 경우, iframe에서 `parent.phoneNumber`로 이 변수를 참조할 수 있다.

## 상수

`const` 키워드로 읽기 전용 상수를 만들수 있다.

```javascript
const prefix = '212';
```

스크립트가 실행 중인 동안 대입을 통해 값을 바꾸거나 재선언될 수 없고 값으로 초기화 해야한다. 상수는 같은 범위에 있는 함수나 변수와 동일한 이름으로 선언할 수 없음.

```javascript
function f() {};
const f = 5;
// 오류 발생

function f() {
    const g = 5;
    var g;
};
// 오류 발생
```

## 데이터 구조 및 형

### 데이터 형

최신 ECMAScript 표준은 7가지 데이터 형을 정의합니다.

- 6가지 원시 데이터 형
  - Boolean. true와 false
  - null. (javascript는 대소문자를 구분하므로 Null, NULL과는 다름.)
  - undefined. undefined 값인 최상위 속성
  - Number.
  - String.
  - Symbol. (ECMAScript 6에 도입) 인스턴스가 고유하고 불변인 데이터 형.
- Object

### 데이터 형 변환

javascript는 동적 형지정(정형)언어; 변수를 선언할 때 데이터 형을 지정할 필요가 없고, 데이터 형이 스크립트 실행 도중 필요에 의해 자동으로 변환됨.

```javascript
var answer = 42;
answer = "Thanks for all the fish";
```

숫자와 문자열 값 사이에 + 연산자를 포함한 식에서, javascript는 숫자 값을 문자열로 변환

```javascript
x = "The answer is " + 42; // "The answer is 42"
```

다른 연산자를 포함한 식의 경우, 숫자 값을 문자열로 변환하지 않음

```javascript
"37" - 7 // 30
"37" + 7 // 377
```

### 문자열을 숫자로 변환하기

숫자를 나타내는 값이 문자열로 메모리에 있는 경우, 변환을 위한 메서드가 있음.

- `parseInt()`
- `parseFloat()`

`parseInt`는 오직 정수만 반환하고 진법(Radix) 매개변수를 포함해야 함

문자열을 숫자로 변환하는 대안은 +(단항 더하기) 연산자

```javascript
"1.1" + "1.1" // "1.11.1"
(+"1.1") + (+"1.1") // 2.2
```

## 리터럴

### 배열 리터럴

배열 리터럴은 0개 이상의 식(expression) 목록이다. 각 식은 배열 요소를 나타내고 대괄호([])로 묶인다. 배열 리터럴을 사용하여 배열을 만들 때, 그 요소로 지정된 값으로 초기화되고, 그 길이는 지정된 인수의 갯수로 설정됨.

```javascript
var coffees = ["French Roast", "Colombian", "Kona"];
```

***Note**: 배열 리터럴은 일종의 객체 이니셜라이저(initializer)입니다.*

배열이 최상단 스크립트에서 리터럴을 사용해서 만들어진 경우, javascript는 배열 리터럴을 포함한 식을 평가할때 마다 배열로 해석하고, 함수에서 사용되는 리터럴은 함수가 호출될 때마다 생성됨.

#### 배열의 추가 쉼표

```javascript
var fish = ["Lion", , "Angle"];
fish[1]; // undefined
```

만약 후행 쉼표로 끝낸다면 그 쉼표는 무시되나 구 브라우저에서 오류를 일으킬수 있으므로 제거하는 것이 최선이다

```javascript
var myList = ["home", , "school",];
myList[3]; // undefined
```

코드를 작성할 때는 빠진 요소의 값을 명시적으로 undefined로 선언하는 것이 코드의 명확성과 유지보수성을 높입니다.

### 불린 리터럴

true/false

### 정수

정수는 10진, 16진, 8진, 2진수로 표현될 수 있음

```text
0, 117 및 -345 (10진수)
015, 0001 및 -0o77 (8진수)
0x1123, 0x00111 및 -0xF1A7 (16진수)
0b11, 0b0011 및 -0b11 (2진수)
```

### 부동 소수점 리터럴

다음 4가지 요소로 구성됩니다.

- 부호가 달릴 수 있는 10진 정수
- 소수점
- 소수
- 지수

```text
[(+|-)][digits][.digits][(E|e)[(+|-)]digits]
ex.
3.1415926
-.123456789
-3.1E+12
.1e-23
```

### 객체 리터럴

중괄호({})로 묶인 0개 이상인 객체의 속성명과 관련 값 쌍 목록.

```javascript
var sales = "Toyota";

function carTypes(name) {
  if (name === "Honda") {
    return name;
  } else {

  }
    return "Sorry, we don't sell " + name + ".";
}

var car = { myCar: "Saturn", getCar: carTypes("Honda"), special: sales };

console.log(car.myCar);   // Saturn
console.log(car.getCar);  // Honda
console.log(car.special); // Toyota
```

속성명으로 숫자나 문자열 리터럴을 사용하거나 또다른 객체 리터럴 내부에 객체를 중첩할 수도 있다.

```javascript
var car = { manyCars: {a: "Saab", "b": "Jeep"}, 7: "Mazda" };

console.log(car.manyCars.b); // Jeep
console.log(car[7]); // Mazda
```

속성명이 유효한 식별자가 아닌명은 점(.) 속성으로 접근할 수 없다.

### 정규식 리터럴

슬래시 사이에 감싸진 패턴

```javascript
var re = /ab+c/;
```

### 문자열 리터럴

큰 따옴표(") 또는 작은 따옴표(')로 묶인 0개 이상의 문자

문자열 리터럴 값은 문자열 객체의 모든 메서드를 호출할 수 있다. 이때 javascript는 자동으로 문자열 리터럴을 임시 문자열 객체로 변환, 메서드를 호출하고 나서 임시 문자열 객체를 폐기한다.

ES2015에서는 탬플릿 리터럴도 사용 가능하다. 탬플릿 리터럴은 내장된 표현식을 허용하는 문자 리터럴으로 백틱(`) 을 사용한다. [자세한 내용](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Template_literals)
