# 3. 제어 흐름과 에러 처리

## Block 문

중괄호({})에 의해 범위가 결정

```javascript
{
    statement_1;
    statement_2;
    statement_n;
}
```

ECMAScript6 이전의 javascript는 블록 범위를 가지지 않는다. Block 내에서 선언한 변수는 블록을 넘어 변수가 위치한 함수 혹은 스크립트에 영향을 끼친다. 즉, block은 변수의 범위를 지정하지 않음.

```javascript
var x = 1;
{
    var x = 2;
}
console.log(x); // output 2
```

ECMAScript 6부터 let 변수 선언으로 변수의 블록 범위를 제한할 수 있다.

## 조건문

### if...else문

```javascript
if (condition) {
    statement_1;
} else {
    statement_2;
}
```

여러 조건을 중첩해서 사용 가능하다.

```javascript
if (condition_1) {
    statement_1;
} else if (condition_2) {
    statement_2;
} else if (condition_n) {
    statement_n;
} else {
    statement_last;
}
```

동등비교연산자로 오해할 수 있기 때문에, 조건문 안에서 변수값 할당은 사용하지 않는 것이 좋다.

```javascript
// 좋지 않은 예시
if ( x = y ) {
    /* statement here */
}

// 좋은 예시
if (( x = y )) {
    /* statement here */
}
```

#### 거짓으로 취급하는 값

- `false`
- `undefined`
- `null`
- `0`
- `NaN`
- `""` (빈 문자열)

`Boolean` 개체의 참 거짓 값과 원시 boolean 값 true false를 혼동하지 말 것!

```javascript
var b = new Boolean(false);
if ( b ) {
    console.log("this condition evaluates to true");
}
```

### switch문

```javascript
switch (expression) {
    case label_1:
        statements_1
        break;
    case label_2:
        statements_2
        break;
    default:
        statements_def
        break;
}
```

표현식을 평가한 값과 일치하는 case라벨을 찾고 관련된 구문을 수행한다. 만약 매치되는 라벨이 없다면 default 절을 찾는다. defualt절도 없는 경우 switch 문은 종료된다. break 문은 switch문을 벗어나게 하고, break 문이 생략된다면 switch 문안에서 다음 문장을 계속 수행한다.

## 예외처리문

throw 문을 통해 예외를 발생시키고, try...catch 문을 사용하여 예외를 다룰 수 있다.

### 예외 유형

대부분의 자바스크립트 안에서 사용될 수 있으나 특히 아래 예외 유형중 하나를 사용하는데 더 효과적이다.

- [ECMAScript exception](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error#Error_types)
- [DOMException](https://developer.mozilla.org/ko/docs/Web/API/DOMException)과 [DOMError](https://developer.mozilla.org/ko/docs/Web/API/DOMError)

#### throw 문

```javascript
thow expression;
```

예외를 사용할때 객체를 명시할 수 있다. 그리고 나서 `catch` 문안에서 객체의 특성들을 참고할 수 있다.

```javascript
// UserException object 생성
fuction UserException (message) {
    this.message = message;
    this.name = "UserException";
}

// UserException 이 String으로 사용될때 message와 Exception 종류를 예쁘게 보여줌
UserException.prototype.toString = function () {
    return this.name + ': "' + this.message + '"';
}

// object instance 생성 후 throw
throw new UserException("Value too high");
```

### try...catch문법

`try` 블록이 성공하지 않으면 `catch` 블록으로 즉시 넘어가고, 성공하는 경우는 `catch` 블록을 실행시키지 않는다. `finally` 블록은 `try` 블록과 `catch` 블록이 끝나고 `try...catch` 문법 다음의 문장이 실행되기 전에 시행된다.

```javascript
fuction getMonthName (mo) {
    mo = mo - 1;
    var months = ["Jan","Feb","Mar","Apr","May","Jun","Jul",
                "Aug","Sep","Oct","Nov","Dec"];
    if (months[mo] != null) {
        return months[mo];
    } else {
        throw "InvalidMonthNo";
    }
}

try {
    monthName = getMonthName(myMonth);
}
catch (e) {
    monthName = "unknown";
    logMyError(e);
}
```

만약 `finally` 블록이 값을 반환하였을 경우, `try` 블록과 `catch` 블록의 `return` 문장과 상관없이 `try-catch-finally` 생산물의 반환값이 된다. 또한 `try`나 `catch`에서 예외가 발생했을 때도 반환값이 덮어써진다.

### Error 객체를 도구화 하기

더 정제된 메세지를 얻기위해 `name` 속성과 `message` 속성을 사용할 수 있다. `catch` 블록이 시스템의 예외와 직접 작성한 에러를 구분할 때 유용하다.
