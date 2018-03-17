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

