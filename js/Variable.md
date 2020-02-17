### var 가 가진 문제

- 함수 스코프를 가진다.
  - 스코프란 변수가 사용될 수 있는 영역을 의미하며, 스코프는 변수가 정의된 위치에 의해 결정된다. 
- 호이스팅 현상이 일어난다.
  - 호이스팅이란 그 변수가 속한 스코프의 최상단으로 끌어올려지는 현상을 의미.
- 재할당 가능해진다.

<br>

```js
function example() {
  var i = 1
}

console.log(i)
```

i 가 정의되지 않았다는 메세지를 볼 수 있다.

<br>

```js
function ex1() {
  i = 1
}

function ex2() {
  console.log(i)
}

ex1()
ex2()
```

1이 출력된다.

<br>

```js
'use strict'

function ex1() {
  i = 1
}

function ex2() {
  console.log(i)
}

ex1()
ex2()
```

`ReferenceError: i is not defined` 에러 발생

<br>

**use strict 란**

```
The statement "use strict"; instructs the browser to use the Strict mode, which is a reduced and safer feature set of JavaScript.
```

개발자가 실수하기 쉬운 `잘못된 문법`들을 미리 알려주는 역할을 한다고 생각하면 된다고 한다.

<br>

```js
for(var i=0; i<10; i++)
  console.log(i)
console.log(i) 
```

10 이 출력된다.

<br>

```js
console.log(myVar)
var myVar = 10;
```

에러가 발생하기 않고, undefined 가 출력된다.

```js
var myVar = undefined;
console.log(myVar)
myVar = 10;
```

위와 같이 해석을 하기 때문.

<br>

```js
var a = 1;
var a = 2;
```

문제가 되지 않아서 문제이다...

<br>

#### var 의 문제를 해결하는 const, let

**const 와 let 은 block scope 를 갖는다. **

```js
if(true) {
  const i = 0
}
console.log(i)
```

`ReferenceError: i is not defined`

<br>

**호이스팅은 된다. 다만 TDZ (temporal dead zone) 이 존재한다.**

```js
const foo = 1
{
  console.log(foo)
	const foo = 2
}
```

ReferenceError: Cannot access 'foo' before initialization

<br>

```js
const foo = 1
{
  console.log(foo) // 1 이 출력된다.
}
```



<br>

**const 는 재할당 불가능하다**

```js
const a = 'a'
a = 'b' // Error

const b = { p: 'b'}
b.p = 'c' // ok
```

<br>

**블록 스코프 vs 함수 스코프**

```js
/*** 블럭 스코프 ***/
{
    let a = 10;
    {
        let a = 20;
        console.log(a);  // 20
    }
    console.log(a);  // 10
}
console.log(a);  // ReferenceError: a is not defined

/*** 함수 스코프 ***/
(function() {
    var b = 10;
    (function() {
        var b = 20;
        console.log(b); // 20
    })();
    console.log(b); // 10
})();
console.log(b); // ReferenceError: b is not defined
```

<br>

**유명한 문제**

```js
const a = [1, 2, 3, 4, 5]
for(var i=0; i<a.length; i++) {
  setTimeout(function() {
    console.log(`number ${i}`)
  }, 100)
}
```

```js
const a = [1, 2, 3, 4, 5]
for(let i=0; i<a.length; i++) {
  setTimeout(function() {
    console.log(`number ${i}`)
  }, 100)
}
```

두 코드의 결과에 차이가 발생하는 이유..!!

<br>

- 단축 송성명을 사용한 객체
- 계산된 속성명 : 키값을 스트링으로..!
- 전개연산자
- 배열 destructuring
- 객체 destructuring
- 함수의 매개변수 기본값
- 나머지 매개변수
- destructuring 을 활용한 명명된 매개변수
- 화살표 함수
