# DevFest WebTech CodeLab

이 코드랩은 DevFest WebTech 2019 이벤트를 위해 만들어졌습니다. 해마다 조금씩 변화하고 있는 JavaScript의 새로운 명세를 알아보고 실제로 어떻게 사용 및 활용할 수 있을지 함께 알아봅시다!

# CodeLab 과정

이 CodeLab은 아래와 같은 순서로 진행됩니다.

1. ES6와 ES7에 대해서 알아보기
2. ES8, ES9, ES10에 대해서 알아보기
3. ES6~10을 활용해 간단한 검색 기능 만들어보기 feat. RxJS(option)

각 세션은 독립적으로 이루어지니 꼭 순서대로 듣지 않으셔도 되고, 원하는 것만 들으셔도 무방합니다.

# ECMAScript?

현재 우리가 쓰고있는 JavaScript는 정보와 통신 시스템을 위해 존재하는 국제 표준화 기구 Ecma International의 ECMA-262 기술 규격에 정의 및 표준화된 ECMAScript와 BOM(Browser Object Model) 그리고 DOM(Document Object Model) 이 세 가지로 이루어져 있어요. 이 중 ECMAScript는 JavaScript의 표준 규격을 담당하고 2015년부터 약 1년을 주기로 명세가 새롭게 추가 및 수정되면서 업데이트 되고있어요.

# ES6 & ES7에서 무엇이 달라졌나요?

ES6부터 현재 2019년도에 나온 ES10까지 ECMAScript의 버전 중 ES6에서 가장 많은 변화가 생겼어요. 약 20개 이상의 명세가 추가되었는데 그 중 대표적인 변화를 알아보고 프로덕트에서 유용하게 사용할 수 있는 것들을 직접 활용해 봅시다.

## ES6 → [http://www.ecma-international.org/ecma-262/6.0/](http://www.ecma-international.org/ecma-262/6.0/)

## Arrow functions / Arrow function expressions

→ Regular function 대신 사용할 수 있는 문법적으로 간소화된 함수

→ 주의할 점: 이 Arrow function은 regular function 과 동일한 방식으로 `this`, `arguments`, `super`, `new.target` keyword에 대한 바인딩이 되지 않습니다.

    // 두 함수의 생김새
    
    // Regular Function
    function generalFunction(a, b) {
      // ...
    }
    
    // Arrow Function
    const arrowFunction = (a, b) => {
      // ...
    }

### 어떻게 활용할 수 있나요?

    const cats = ['ebony', 'ivory', 'harmony', 'rhythm']
    
    // Statement body
    cats.forEach((cat) => {
    	console.log(cat)
      registerCat(cat)
    }
    
    // Expression body
    const wrappedCats = cats.map((catName, index) => ({catName, age: index}))
    const olderCats = wrappedCats.filter((cat) => cat.age > 1)
    const catHarmony = olderCats.find((cat) => cat.catName === 'harmony')

참고하면 좋은 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
- [https://hacks.mozilla.org/2015/06/es6-in-depth-arrow-functions/](https://hacks.mozilla.org/2015/06/es6-in-depth-arrow-functions/)

## Classes

→ `Syntactical sugar over JavaScript's existing prototype-based inheritance`

→ JavaScript에 새로운 Object Oriented Pattern을 제시한 건 아니에요!

→ 주의할 점: Class는 Function declaration 처럼 hoisted 되지 않아요.

    class nameOfClass extends existingClass {
      constructor() {
    	  // ...
      }
    
    	someFunction() {
    		// ...
    	}
    
    	get someVariable() {
    		// ...
    	}
    
    	set someVariable(variable) {
    		// ...
    	}
    
    	static someStaticFunction() {
    		// ...
    	}
    }

### 어떻게 활용할 수 있나요?

    class GDGEvent {
    	constructor(eventName, eventDate) {
        this.eventName = eventName
        this.eventDate = eventDate
      }
    
    	updateEvent(eventName, eventDate) {
    		// ...
      }
    }
    
    class GDGWebTechEvent extends GDGEvent {
      constructor(eventName, eventDate, focusingTopic) {
        super(eventName, eventDate)
        this.focusingTopic = focusingTopic
    		this.eventPlace = GDGWebTechEvent.defaultEventPlace()
      }
    
      updateEvent(eventName, eventDate, focusingTopic) {
    		// do something with focusingTopic
        super.updateEvent(eventName, eventDate)
      }
    
      get eventInformation() {
        return `${this.eventName} will be held at ${this.eventDate}
    						focusing on ${this.focusingTopic}`
      }
    
      static defaultEventPlace() {
        return 'Google for Startups located in Seoul, Korea'
      }
    }
    
    const devFestWebTech2019 = new GDGWebTechEvent(
      'DevFest WebTech', '21 Nov 2019', 'web technologies'
    )
    console.log(devFestWebTech2019.eventInformation)
    // 'DevFest WebTech will be held at 21 Nov 2019 focusing on web technologies'

참고하면 좋은 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)
- [https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects#The_object_(class_instance)](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects#The_object_(class_instance))

## Enhanced Object Literals

→ `Object Literals` 가 아래의 내용을 지원하도록 확장되었어요.

→ setting the prototype at construction

→ shorthand for **foo: foo** assignments

→ defining methods(+making super calls)

→ computing property names with expressions

    const exampleObjectLiteral = {
    	// __proto__
    	__proto__: theProtoObj,
    	// Shorthand for ‘handler: handler’
    	handler,
    	// Methods
    	toString() {
    	 // Super calls
    	 return "d " + super.toString()
    	},
    	// Computed (dynamic) property names
    	[ 'prop_' + (() => 42)() ]: 42
    }

### 어떻게 활용할 수 있나요?

    const height = 30
    const width = 60
    const weight = '3kg'
    const color = 'ivory'
    
    const myCat = {
    	height,
    	width,
    	weight,
    	color,
    	[`computed${height * width}`]: height * width
    }

참고하면 좋은 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#Object_literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#Object_literals)

## Template Strings

→ `string literals allowing embedded expressions`

### 어떻게 활용할 수 있나요?

    // Basic literal string creation
    const example1 = `string '/n' text`
    
    // Multiline strings
    const example2 = `string text line 1
     string text line 2`
    
    // String interpolation
    const exmaple3 = `string text ${example1} string text`

    // Tagged templates
    const cat = 'Mambo'
    const age = 8
    
    function simpleTag(strings, catExp, ageExp) {
      const firstString = strings[0]
      const secondString = strings[1]
    
      return `${firstString}${catExp}${secondString}${ageExp > 7 ? 'old' : 'not old'}`
    }
    
    const catString = simpleTag`My cat ${ cat } is ${ age }`
    console.log(catString)

참고하면 좋은 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)

## Destructuring

→ `binding using pattern matching`

### 어떻게 활용할 수 있나요?

    const exampleObject = {
    	animal: {
    		cat: ['Navy', 'Black', 'Yello'],
    		dog: ['Joy', 'Sam'],
    	},
    	car: {
    		sedan: {
    			bmw: ['528i', '530i'],
    			mercedes: ['E300', 'S350D'],
    		},
    		suv: {
    			bmw: ['X3', 'X5'],
    			mercedes: ['GLE', 'GLC'],
    		}
    	}
    }
    
    // list matching
    const [a, , b] = [1,2,3]
    
    // object matching
    const { animal: a, car: { sedan: s } } = exampleObject
    
    // object matching shorthand
    const {animal, car} = exampleObject
    
    // Can be used in parameter position
    function exampleFunction({name: awesomeName}) {
      console.log(awesomeName)
    }
    exampleFunction({name: 'Duke'})
    
    // Fail-soft destructuring
    const [a] = []
    a === undefined
    
    // Fail-soft destructuring with defaults
    const [a = 1] = []
    a === 1

참고하면 좋은 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

## Default parameters & Rest parameters & Spread Operator

→ `Default parameters`: 함수의 선언부에서 파리미터의 기본 값을 지정할 수 있음

→ `Rest parameters`: 함수의 선언부에서 파라미터 일부를 하나의 배열로 전부 받음

→ `Spread Operator`: 배열과 문자열의 요소를 전부 풀어 함수의 인수나 배열의 요소로 확장할 수 있음

### 어떻게 활용할 수 있나요?

    // Default parameters
    function interpretAge(age = 1) {
    	if (age < 3) {
    		return 'baby'
    	}
    
    	return '으른'
    }
    console.log(interpretAge())  // baby
    console.log(interpretAge(1)) // baby
    console.log(interpretAge(5)) // 으른
    
    // Rest parameters
    function organizeRoom(clothes, chairs, ...others) {
    	console.log(clothes) // ['Jeans', 'Shirts']
    	console.log(chairs) // ['a', 'b']
    	console.log(others) // [['Book1', 'Book2'], ['Note1']]
    }
    organizeRoom(['Jeans', 'Shirts'], ['a', 'b'], ['Book1', 'Book2'], ['Note1'])
    
    // Spread Operator
    function sumNumbers(x, y, z, s) {
    	return x + y + z + s
    }
    const numbers = [1, 2, 5, 6]
    sumNumbers(...numbers) // 14

참고하면 좋은 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)
- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

## Let & Const

→ Block scope가 적용되는 변수

→ 주의할 점: `var` keyword로 선언할때 처럼 `let` 및 `const` 로 선언되는 변수는 기본적으로 window 객체의 프로퍼티로 등록되지 않고 다른 공간에 등록됩니다. var는 Object Environment Record, let과 const는 Declarative Environment Record 로서 서로 다르게 취급됩니다.

### 어떻게 활용할 수 있나요?

    // 일반 함수에서의 쓰임새
    function f() {
      {
        let x
        {
          // 다른 블록 스코프라서 문제없이 실행
          const x = 'sneaky'
          // 같은 블록 스코프에서 const 변수를 재할당하므로 error
          x = 'foo'
        }
        // 이미 같은 블록 스코퍼에 동일한 이름의 let 변수가 선언되어 있어 error
        let x = 'inner'
      }
    }
    
    // 고전적이면서도 유명한 예
    for (var i = 0; i < 10; i++) {
    	setTimeout(() => {
    		console.log(i)
    	}, 3000)
    }
    // 열번 모두 10
    
    // Versus
    
    for (let i = 0; i < 10; i++) {
    	setTimeout(() => {
    		console.log(i)
    	}, 3000)
    }
    // 0 ~ 9

참고하면 좋은 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
- [https://stackoverflow.com/questions/55030498/why-dont-const-and-let-statements-get-defined-on-the-window-object](https://stackoverflow.com/questions/55030498/why-dont-const-and-let-statements-get-defined-on-the-window-object)
- [https://www.ecma-international.org/ecma-262/9.0/index.html#sec-declarative-environment-records](https://www.ecma-international.org/ecma-262/9.0/index.html#sec-declarative-environment-records)

## Modules

→ `Language-level support for modules for component definition`

→ 드디어 언어 레벨에서 지원되는 import, export 구문

→ 주의할 점: 아직 브라우저에서 완전히 지원하지는 않아서 script tag의 type 속성에 `module` 을 지정해주어야 한다.

### 어떻게 활용할 수 있나요?

    // math.js
    export const sum = (x, y) => x + y
    
    export const subtract = (x, y) => x - y
    
    export const multiply = (x, y) => x * y
    
    export const PI = 3.141593
    
    export default {
    	sum,
    	subtract,
    	multiply,
    	PI,
    }
    
    // app.js
    import math from 'math'
    console.log('2π = ' + math.sum(math.PI, math.PI))
    
    // or
    import { sum, multiply, PI } from 'math'
    console.log(multiply(sum(3, 8), sum(2, 10)) / PI) // 42.01...

    <script type="module" src="app.js"></script>

참고하면 좋은 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)

## Array API: from, of, fill, find ...

→ Array에 여러 conversion helpers가 추가되었어요!

### 어떻게 활용할 수 있나요?

    // Array-like DOM Nodes를 실제 배열 타입으로 래핑
    Array.from(document.querySelectorAll('*'))
    
    Array.of(1, 2, 3)
    
    // 7로 1번째부터 채우기
    [0, 0, 0].fill(7, 1) // [0, 7, 7]
    
    // 특정 요소 및 인덱스 찾기
    [1, 2, 3].find(x => x === 3) // 3
    [1, 2, 3].findIndex(x => x === 2) // 1

참고하면 좋은 문서

- [https://developer.mozilla.org/ko/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_6_support_in_Mozilla](https://developer.mozilla.org/ko/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_6_support_in_Mozilla)

## Promise

→ JavaScript의 비동기 코드를 일관적인 방법으로 다룰 수 있도록 도와주는 라이브러리로 오픈소스로 관리되고 있다가 ES6에 이르러서 JavaScript 명세로 추가되었어요.

    new Promise((resolve, reject) => { ... })

### 어떻게 활용할 수 있나요?

    // 타이머
    function timeout(duration = 0) {
      return new Promise((resolve) => {
    	  setTimeout(resolve, duration);
      })
    }
    
    const promiseExample = timeout(1000)
    	.then(() => {
        return timeout(2000)
    	})
    	.then(() => {
        throw new Error('hmm')
    	})
    	.catch((error) => {
    		Promise.all([timeout(100), timeout(200)])
    		return '마이 기다렸다..'
    	})
    
    // API를 호출하는 경우
    fetch('https://awesome.com/api/cats', { method: 'GET' })
    	.then((response) => response.json())
    	.then(({status, data}) => {
    		// ...
    	})

참고하면 좋은 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

## ES7 → [http://www.ecma-international.org/ecma-262/7.0/](http://www.ecma-international.org/ecma-262/7.0/)

## Array.prototype.includes

→ 기존의 `indexOf` 는 특정 요소가 있는지 검사하고 0 또는 1을 반환했지만, `includes` 는 동일하게 요소를 검사하고 boolean을 반환해요. 그래서 number type을 boolean type으로 conversion 하는 일을 안하게 되었어요.

### 어떻게 활용할 수 있나요?

    // arrayVariable.includes(valueToFind[, fromIndex])
    
    [1, 2, 3].includes(-1)                   // false
    [1, 2, 3].includes(1)                    // true
    [1, 2, 3].includes(3, 4)                 // false
    [1, 2, 3].includes(3, 3)                 // false
    [1, 2, NaN].includes(NaN)                // true
    ['foo', 'bar', 'quux'].includes('foo')   // true
    ['foo', 'bar', 'quux'].includes('norf')  // false

참고하면 좋은 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)

## Arithmetic Operators: Exponentiation(**)

→ 수를 제곱하는 Operator가 추가되었어요.

### 어떻게 활용할 수 있나요?

    // 5의 세제곱을 구할때 기존의 방법
    Math.pow(5, 3)
    
    // 이제는 operand ** operand 이렇게 사용할 수 있어요
    5 ** 3 // 125

참고하면 좋은 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Exponentiation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Exponentiation)

# ES8 & ES9에서 무엇이 달라졌나요?

## ES8 → [http://www.ecma-international.org/ecma-262/8.0/](http://www.ecma-international.org/ecma-262/8.0/)

## Async function

→ async를 붙이는 것을 제외하고는 Regular function과 구조적으로 동일하지만 암묵적으로 Async function은 Promise를 반환하고, 이 함수 내에서는 await operator로 Promise 혹은 Thenable objects를 기다렸다가 fulfilled 된 값을 받을 수 있어요.

    async function asynchronousFunction() {
    	return 3
    }
    
    const resultAsPromise = asynchronousFunction()

### 어떻게 활용할 수 있나요?

    // 기본적인 사용 방법
    async function getFive() {
    	return 5
    }
    
    getFive().then((value) => {
    	console.log(value)
    })
    
    // or
    
    async function getFive() {
    	return 5
    }
    
    async function waitForFive() {
    	const value = await getFive()
    	console.log(value)
    }
    
    waitForFive()
    
    // Thenable objects를 기다릴때
    async function waitForThenable() {
      const thenable = {
        then: function(resolve, reject) {
          resolve('resolved!')
        }
      }
      console.log(await thenable) // resolved!
    }
    waitForThenable()
    
    // API를 호출하는 경우
    
    // Promise로 했을때(ES6에서의 예제와 동일)
    fetch('https://awesome.com/api/cats', { method: 'GET' })
    	.then((response) => response.json())
    	.then(({status, data}) => {
    		// ...
    	})
    
    // Async function과 await을 사용했을때
    async function apiHandler() {
    	const response = await fetch('https://awesome.com/api/cats', { method: 'GET' })
    	const { data } = await response.json()
    	console.log(data)
    }
    apiHandler()

참고하면 좋은 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)
- [https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)

## Object entries, values

→ Object의 enumerable property pair를 꺼내오거나 value만 배열로 꺼내올 수 있어요.

    Object.entries(objectVariable)
    Object.values(objectVariable)

### 어떻게 활용할 수 있나요?

    // Object.entries()
    const obj = { eventName: 'DevFest WebTech', eventPlace: 'Google for Startups' }
    console.log(Object.entries(obj))
    // [ ['eventName', 'DevFest WebTech'], ['eventPlace', 'Google for Startups'] ]
    
    // Object.values()
    const obj = { foo: 'foo', bar: [100, 200], baz: 55 }
    console.log(Object.values(obj))
    // ['foo', [100, 200], 55 ]
    
    const myStr = 'Lufthansa'
    console.log(Object.values(myStr))
    // ["L", "u", "f", "t", "h", "a", "n", "s", "a"]

참고하면 좋은 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/values](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/values)

## Trailing commas in function declarations and calls

→ Trailing commas를 함수 선언부와 호출부에서도 사용할 수 있게 되었어요.

→ 이 문법으로 이제는 Array, Object, Function 등을 활용함에 있어서 실제 프로덕트를 만들때 자주 하는 열거형 값 복사를 Syntax Error나 Code Convention(ESLint 혹은 Prettier을 사용한다면) 등을 걱정하지 않고 할 수 있게 되었어요!

    // ES5에서도 legal한 문법이었던 trailing commas in objects
    const arr = [
      1, 
      2, 
      3, 
    ];
    
    const exampleObject = { 
      foo: "bar", 
      baz: "qwerty",
      age: 42,
    };
    
    // 이제는 함수의 선언부와 호출부에서도 legal!
    const awesomeFunction = (
    	a,
    	b,
    	c,
    	d,
    ) => a + b + c + d
    
    const a = 3
    const b = 22
    const c = 1
    const d = 5
    awesomeFunction(a, b, c, d,)

참고하면 좋은 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Trailing_commas](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Trailing_commas)

## ES9 → [http://www.ecma-international.org/ecma-262/9.0/](http://www.ecma-international.org/ecma-262/9.0/)

## Object rest & spread

→ 앞서 설명했던 Array rest와 spread 연산자가 Object에도 사용할 수 있게 되었어요. 

### 어떻게 활용할 수 있나요?

    // Object rest
    const { fname, lname, ...rest } = {
    	fname: 'Hemanth',
    	lname: 'HM',
    	location: 'Earth',
    	type: 'Human',
    }
    
    fname // "Hemanth"
    lname // "HM"
    rest // { location: "Earth", type: "Human" }
    
    // Object spread
    const rest = {
    	location: 'Earth',
    	type: 'Human',
    }
    const info = { fname, lname, ...rest }
    
    info // { fname: "Hemanth", lname: "HM", location: "Earth", type: "Human" }

참고하면 좋은 문서

- [https://medium.com/@bramus/javascript-whats-new-in-ecmascript-2018-es2018-17ede97f36d5](https://medium.com/@bramus/javascript-whats-new-in-ecmascript-2018-es2018-17ede97f36d5)

## Promise.prototype.finally

→ 앞서 설명했던 Promise에 then, catch 다음으로 finally 구문을 실행할 수 있게 되었어요. 이젠 then과 catch에서 했던 clean up 작업을 finally 구문에서 일괄적으로 처리할 수 있어요.

### 어떻게 활용할 수 있나요?

    function testFinally() {
      return new Promise((resolve,reject) => resolve())
    }
    
    testFinally()
    	.then(() => console.debug('resolved'))
    	.finally(() => console.debug('finally'))
    // resolved
    // finally

참고하면 좋은 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally)

## ES10 → [http://www.ecma-international.org/ecma-262/10.0/](http://www.ecma-international.org/ecma-262/10.0/)

## Array flat & flatMap

→ Array에 배열의 배열을 flat 시킬 수 있는 conversion helper가 추가 되었어요.

### 어떻게 활용할 수 있나요?

    // Array flat
    let arr = ['a', 'b', ['c', 'd']]
    let flattened = arr.flat()
    console.log(flattened)    // ["a", "b", "c", "d"]
    
    arr = ['a', , , 'b', ['c', 'd']];
    flattened = arr.flat()
    console.log(flattened)    // ["a", "b", "c", "d"]
    
    arr = [10, [20, [30]]]
    console.log(arr.flat())     // [10, 20, [30]]
    console.log(arr.flat(1))    // [10, 20, [30]]
    console.log(arr.flat(2))    // [10, 20, 30]
    
    // Array flatMap
    const arr = [4.25, 19.99, 25.5]
    console.log(arr.map((value) => [Math.round(value)]))
    // [[4], [20], [26]]
    console.log(arr.flatMap((value) => [Math.round(value)]))
    // [4, 20, 26]

참고하면 좋은 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap)

## Object fromEntries

→ 이전에 설명했었던 Object의 entries()로 리터럴 객체의 property pair를 배열로 가져올 수 있었어요. 그런데 반대로 그 배열로 리터럴 객체를 만들 수 있는 함수가 생겼어요.

### 어떻게 활용할 수 있나요?

    const myArray = [['one', 1], ['two', 2], ['three', 3]]
    const obj = Object.fromEntries(myArray)
    
    console.log(obj)    // {one: 1, two: 2, three: 3}

참고하면 좋은 문서

- [https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/fromEntries](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/fromEntries)

## String trimStart & trimEnd

→ 사실 이 String의 함수들은 각각 trimLeft와 trimRight와 같아요.

### 어떻게 활용할 수 있나요?

    const str = '   string   '
    
    // ES10
    console.log(str.trimStart())    // "string   "
    console.log(str.trimEnd())      // "   string"
    
    // the same as
    console.log(str.trimLeft())     // "string   "
    console.log(str.trimRight())    // "   string"

참고하면 좋은 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/trimStart](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/trimStart)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/trimEnd](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/trimEnd)

## Optional catch binding

→ 이제 catch binding을 생략할 수 있게 되었어요.

### 어떻게 활용할 수 있나요?

    // 이제 이렇게 catch가 어떤 Error 객체를 받던지 생략하고 필요한 작업을 수행할 수 있어요!
    try {
      // 에러가 날 가능성이 있는 작업들을 수행
    } catch {
      // 에러의 종류에 상관없이 필요한 작업을 수행
    }

참고하면 좋은 문서

- [https://github.com/tc39/proposal-optional-catch-binding](https://github.com/tc39/proposal-optional-catch-binding)

# 간단한 검색 기능 만들어보기 feat. RxJS

### 준비물

- Editor(or IDE)
- npm
- Browser(Chrome을 권장합니다)

### 무엇을 만드나요?

이번 세션에는 앞서 살펴보았던 ES6부터 10까지 일부 기능을 이용하고 공개 되어있는 Public API(음식 레시피 API)를 사용해서 간단한 검색 UI를 만들어 볼거에요. 먼저 Vanilla JS로 만들어보고, 그 다음 RxJS를 활용해 코드를 컨버팅 해볼게요.

### 어떻게 따라하면 되나요?

이번 세션은 앞선 세션과 달리 API 호출을 하고 외부 라이브러리를 npm으로 가져와 웹을 만들어 볼 것이기 때문에 HTML, CSS, JS 파일을 하나씩 만들고, JS 파일은 webpack으로 빌드해서 index.html에서 사용하는 방법으로 진행하게 될 거에요. 대략적인 구조는 아래와 같습니다.

- 프로젝트 폴더에 index.html, app.js, app.css 파일이 있다.
- webpack으로 app.js 파일을 빌드해서 index.html에서 불러와 사용한다.
- index.html을 로컬에서 서빙한다.
    - Ex) npx http-server
- 브라우저에서 로컬호스트로 접속해 웹 애플리케이션을 확인한다.

먼저 프로젝트를 위한 폴더를 원하는 이름으로 만들어주세요.

    mkdir awesome-search
    cd awesome-search

그리고 프로젝트 폴더 안에서 패키지 관리를 위해서 npm init을 해주세요.

    npm init

검색 기능을 만들기 위해서 lodash를 사용해볼게요. npm으로 lodash를 설치해주세요. 원하는 라이브러리로 만들고 따라하셔도 됩니다!

    npm install lodash-es --save

필요한 라이브러리를 설치했으니 이제 HTML 파일을 만들어볼게요.

    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <link rel="stylesheet" href="./app.css">
      <title>DevFest WebTech 2019 CodeLab</title>
    </head>
    <body>
      <div class="container">
        <header>
          Find your recipes!
        </header>
        <main>
          <input class="searchInput" type="text">
          <ul class="recipeList"></ul>
        </main>
      </div>
      <script src=""></script>
    </body>
    </html>

뭔가 초라해 보이죠? CSS로 원하는대로 디자인 해주세요! 이제 JS 파일을 만듭시다. 이름은 아무렇게 만들어주세요.

    // app.js
    
    const searchInput = document.querySelector('.searchInput')
    const recipeList = document.querySelector('.recipeList')
    
    const inputHandler = (event) => {
    	// ...
    }
    
    searchInput.addEventListener('input', inputHandler)

우리의 목표는 input tag에서 문자열을 입력받아 원하는 타이밍에 API를 호출하고, 응답 결과를 ul tag에 보여주는 것이니 초기엔 아마 이런 모습을 하고있겠죠?

그런데 매번 input tag에서 키가 입력될 때마다 이벤트 핸들러가 호출이 될거에요. 이 콜백 함수 안에서 API 호출을 할텐데 키가 입력될 때마다 호출이 일어나면 Network IO가 너무 지나치게 일어나니 낭비를 아래처럼 줄여볼게요.

    import { debounce } from 'lodash-es'
    
    const debouncedInputHandler = debounce((event) => {
    	// ...
    }, 1000)

이렇게 기존의 이벤트 핸들러를 lodash의 debounce에 넣어주면, debounce는 내부적으로 setTimeout을 활용해 두번째 인자로 넘어온 밀리세컨드만큼의 시간안에서 가장 마지막에 발생한 event를 반환하는 새로운 함수를 반환해요. 다시 말해, 이 이벤트 핸들러는 이제 키보드를 연속으로 타이핑 했을때 1초 미만의 간격으로 타이핑돼서 넘어오는 event는 무시하게 될 거에요.

자, 이젠 우리가 원하는 타이밍에 API를 호출할 수 있게 되었으니 API 호출을 해볼까요?

    fetch('some url', {
      method: 'GET',
    })
    .then((response) => ...)

브라우저에 내장되어 있는 fetch 함수를 사용했어요. 기존에 있던 XMLHttpRequest과는 형태와 사용 방법이 사뭇 다를 거에요. fetch는 비동기적으로 작동하고 Promise를 반환해요. 그래서 이 fetch는 `async await` 과도 함께 사용할 수 있어요.

이제 데이터를 잘 받아왔으니 리스트 형태로 그려봅시다.

    const apiEndPoint = 'https://www.themealdb.com/api/json/v1/1/search.php'
    
    fetch(`${apiEndPoint}?f=${inputValue}`, {
      method: 'GET',
    })
    .then((response) => response.json())
    .then(({ meals }) => {
      recipeList.innerHTML = meals
        .map((meal) => ...)
    })

이때 meals 배열 데이터를 바로 DOM으로 바꿔서 그려야 하는데, 다양한 방법이 있을 것 같아요. JSON을 html string으로 전환해주는 유틸 함수를 만들어서 map에 적용해도 될 것 같고, 직접 그 내용을 콜백 함수에 적어도 될 것 같아요. 이번에는 이 meal 하나를 표현할 Class를 새로운 meal.js 라는 파일에 만들어서 데이터를 그려볼게요. 만드는 방법은 자유이니 그냥 보기만 하셔도 됩니다!

    class Meal {
      constructor(mealData) {
        this.mealData = mealData
      }
    
      renderToString() {
        const {
          strArea,
          strCategory,
          strIngredient1,
          strMeasure1,
          strInstructions,
          strMeal,
          strMealThumb,
          strTags,
        } = this.mealData
    
        return `
          <li class="recipe">
            <div>
              <img
                width="130px"
                height="130px"
                src=${strMealThumb}
              >
            </div>
            <div class="recipeColumn">
              <span>${strArea}</span> <span>${strCategory}</span>
              <br />
              <span>${strMeal}</span>
              <br /><br />
              Ingredient List
              <ul>
                <li>${strIngredient1}: ${strMeasure1}</li>
              </ul>
              <br />
              Instruction
              <div class="recipeInstructions">${strInstructions}</div>
              <br /><br />
              <span>${strTags}</span>
            </div>
          </li>
        `
      }
    }
    
    export default Meal

이 Meal 클래스는 인스턴스로 만들어질때 자신을 표현할 데이터를 받고, 나중에 자신이 갖고 있는 데이터로 화면에 그려질 수 있도록 내부적으로 renderToString 이라는 함수를 갖고 있어요. 정말 간단한 클래스네요! 이제 이 클래스를 활용해서 아까의 fetch 함수에서 데이터를 그려볼게요. 아! 이 클래스는 meal.js 라는 파일에 작성되어 있으니 app.js에서 이 파일을 가져와야겠죠?

    import Meal from './meal'

Meal 클래스는 default로 export 되어있으니 이렇게 가져오면 될 거에요.

자, 이제 정말 그려봅시다.

    fetch(`${apiEndPoint}?f=${inputValue}`, {
      method: 'GET',
    })
    .then((response) => response.json())
    .then(({ meals }) => {
      recipeList.innerHTML = meals
        .map((meal) => new Meal(meal))
        .map((mealInstance) => mealInstance.renderToString())
        .join('')
    })

이렇게 보니 new 키워드로 인스턴스화 할뿐, 함수로 작성한다고 했을때와 크게 달라보이지는 않죠? 중요한 건, 우리가 만들 웹 애플리케이션에서 주로 다룰 데이터를 표현하고, 이 데이터가 어떻게 다뤄져야 하는지에 대해서 코드로 명시했다는 점이에요. 그것은 함수로 해도 되고, 클래스로 해도 돼요. ~~물론 저는 함수를 선호합니다..~~

자 이제 여기까지 했으면 input tag에 타이핑을 하다가 멈추면 검색 결과가 화면에 그려질 거에요! 그런데, 만약 apple pie로 검색했다가, 타이핑을 다시 막 했는데 결국 최종 입력 값은 apple pie로 이전과 똑같다면 어떻게 될까요? 지금은 똑같은 query로 API가 호출될 거에요. 뭔가 낭비가 생긴 것 같아요. 이전에 검색했던 값을 어딘가에 저장해 두었다가 바뀌었을때만 API를 호출해봅시다. 그리고 아무것도 입력되지 않은 경우에도 API가 호출되지 않도록 막아봅시다.

    let previousInputValue = ''
    
    const debouncedInputHandler = debounce((event) => {
    	if (!inputValue || previousInputValue === inputValue) {
    	  return
    	}
    
    	fetch(`${apiEndPoint}?f=${inputValue}`, {
    	  method: 'GET',
    	})
    	.then((response) => response.json())
    	.then(({ meals }) => {
    	  recipeList.innerHTML = meals
    	    .map((meal) => new Meal(meal))
    	    .map((mealInstance) => mealInstance.renderToString())
    	    .join('')
    	})
    	
    	previousInputValue = inputValue
    }, 1000)

자, 이렇게 하면 이제 값이 없거나, 이전에 검색했던 값은 건너뛰게 되었어요. 뭔가 조금은 개선된 것 같아요!

그런데 만약 API가 어떤 이유로 에러를 내게 되면 어떻게 될까요? 우리의 코드는 아직 에러를 받아들일 준비가 되지 않았어요. 다행히도 우리는 Promise를 반환하는 fetch를 사용하고 있기 때문에 아래처럼 핸들링 할 수 있을 거에요. 물론 try catch 구문을 사용해도 됩니다.

    .then(({ meals }) => {
      recipeList.innerHTML = meals
        .map((meal) => new Meal(meal))
        .map((mealInstance) => mealInstance.renderToString())
        .join('')
    })
    .catch(() => {
      recipeList.innerHTML = '<h3>An error has occurred when fetching data...</h3>'
    })

흠, 그런데 검색 API가 도착하기 전까지는 데이터가 로딩되고 있다고 표시해주면 좋을 것 같아요. 그 표시는 언제해야 할까요? API를 호출하자마자 보여주고, API가 잘 도착했거나 실패한 이후에 알맞은 결과를 보여주면 되지 않을까요?

    recipeList.innerHTML = '<h3>Loading...</h3>'
    
    fetch(`${apiEndPoint}?f=${inputValue}`, {
      method: 'GET',
    })
    .then(({ meals }) => {
      recipeList.innerHTML = meals
        .map((meal) => new Meal(meal))
        .map((mealInstance) => mealInstance.renderToString())
        .join('')
    })
    .catch(() => {
      recipeList.innerHTML = '<h3>An error has occurred when fetching data...</h3>'
    })

이미 API가 잘 도착한 상황과 실패한 상황 모두를 then과 catch에서 잡아 적절한 표시를 해주고 있어서 우리는 그냥 fetch 하기 직전에 바로 Loading Indicator를 보여주면 돼요!

자 그럼 이제 Loading Indicator도 있고 에러도 핸들링할 수 있어서 더 구색이 갖추어졌어요. Promise를 맛보았으니 이전 세션에서 보았던 async await를 사용해봅시다.

    const debouncedInputHandler = debounce(async ({ target: { value: inputValue } }) => {
      if (!inputValue || previousInputValue === inputValue) {
    	  return
    	}
    
      recipeList.innerHTML = '<h3>Loading...</h3>'
    
      try {
        const response = await fetch(`${apiEndPoint}?f=${inputValue}`, {
          method: 'GET',
        })
        const { meals } = await response.json()
    
        recipeList.innerHTML = meals
          .map((meal) => new Meal(meal))
          .map((mealInstance) => mealInstance.renderToString())
          .join('')
      } catch {
        recipeList.innerHTML = '<h3>An error has occurred when fetching data...</h3>'
      }
    
      previousInputValue = inputValue
    }, 1000)

async await로 만든 동일한 버전의 코드에요. 이전과 달라진 점은 debounce에 넘기는 이벤트 핸들러 콜백 함수가 async로 선언되어 있다는 것, Promise를 반환하는 코드 앞에 await가 쓰여 마치 동기적인 코드로 보이는 것, Promise를 반환하는 코드 전체를 try catch 구문이 감싸고 있는 것이에요. 에러는 여기서 잡아도 되고 더 상위 공간에서 잡아도 괜찮아요. 그것은 앱을 어떻게 설계하느냐에 달려있답니다.

자, 이제 Vanilla JS로는 다 만들어 보았으니 이번엔 RxJS를 활용해서 동일한 웹 애플리케이션을 만들어 봅시다. 시작하기에 앞서, 먼저 RxJS에서 주로 사용되는 개념에 대해서 아주 간단하게 알아볼게요.

### Observable

우리가 애플리케이션을 만들때 데이터는 API에서도 오고, 여러 DOM tag에 달린 이벤트 리스너에서도 오겠죠? 하지만 언제 올지는 아무도 모르죠. 그래서 우리는 이벤트 리스너를 Browser에 `등록` 을 해서 어떤 이벤트가 발생하는지 Browser가 대신 우리가 필요한 작업(콜백 함수)을 실행해 주죠. API도 마찬가지에요. 언제 올지 모르는 비동기성 호출을 도착했을때 적절하게 처리하기 위해 Promise로 API를 핸들링 했어요.

RxJS는 이런 모든 종류의 데이터의 흐름을 표현해주는 객체랍니다. 정확히는 `시간을 축으로 연속적인 데이터를 저장하고 있는 객체` 입니다. 우리가 input tag에 awesome이라고 타이핑을 하면 a → aw → awe → awes → aweso → awesom → awesome 순으로 데이터가 넘어옵니다. API들도 호출한 순서에 따라 컨트롤해야 하는 필요성이 있죠? Observable은 이렇게 시간순으로 오는 데이터를 의미한다고 이해하시면 됩니다. 

### Operator

가장 중요한 Observable을 이해했으니 이건 좀 더 쉽습니다. 이렇게 시간순으로 연속적으로 들어오는 데이터를 우리는 그대로 사용하기도 하지만 계산을 하거나 log를 남기기도 하죠. 이렇게 데이터의 흐름 사이 사이에 어떤 특정한 `작업` 을 해야하는데, 이 작업을 해줄 수 있게 해주는 녀석이 바로 Operator 에요(Operator는 Observable을 만들어내기도 합니다). 좀 더 정확히는, 이 Operator는 Observable을 받아 어떤 작업을 하고 새로운 Observable을 뒤로 넘깁니다. 그럼 다음 Operator가 받겠죠!

### Observer

이렇게 Observable과 Operator로 데이터가 어떻게 흘러서 어떻게 변하는지를 표현했으니, 이 데이터를 결국엔 어딘가에서 꺼내서 활용을 해야겠죠? 그 활용을 하는 녀석이 바로 Observer에요. 그래서 관찰자인거죠! 뒤에 코드로 보면 더 쉽게 이해가 가겠지만, 더 정확히 설명하면 Observer는 next, error, complete 함수를 프로퍼티로 갖는 객체를 의미해요. 이 객체를 `observableInstance.subscribe()` 이렇게 subscribe 함수 안에 넣어주면 데이터를 꺼내오기 시작해요.

### Subscription

Observer가 데이터를 `observableInstance.subscribe()` 로 꺼내오기 시작했어요. 그런데 이렇게 언제 올지 모르는 데이터를 꺼내온다는 것은 RxJS가 Browser 어딘가에 이벤트 핸들러를 달아놨다는 것을 의미하겠죠? 즉, 메모리 자원이 사용되었다는 건데, 더 이상 데이터를 활용하지 않아야 하는 순간이 오면(페이지를 이동하는 등) 사용한 메모리 자원을 다시 반환해야 해요. 그럴 때를 위해 Observable 인스턴스의 subscribe 함수는 subscription 객체를 반환해요. 이 객체를 이용해 `subscription.unsubscribe()` 와 같이 사용한 메모리 자원을 해제할 수 있어요.

이외에도 RxJS에는 Subject, Scheduler 등 개념이 더 있지만 지금은 이 4개만으로 충분합니다! 이 코드랩을 통해 Reactive Programming과 RxJS에 대해 관심을 갖게 되셨다면 저는 기쁩니다! 야호

관심이 있으신 분은 아래 두 링크에서 더 많이 알아보시면 좋겠어요!

- [http://sculove.github.io/blog/2017/10/07/rxjsbook4/](http://sculove.github.io/blog/2017/10/07/rxjsbook4/)
- [https://rxjs-dev.firebaseapp.com/guide/observable](https://rxjs-dev.firebaseapp.com/guide/observable)

개념을 알아보았으니 Vanilla JS로 만들었던 코드를 컨버팅 해봅시다. 무엇부터 해야할까요? 이제는 위에서 설명했던 RxJS의 주요 개념을 이용해서 데이터의 흐름을 표현해야겠죠?

    const inputStream = fromEvent(searchInput, 'input')

`fromEvent` 라는 함수는 DOM EventTarget과 이 타겟에서 데이터로 삼을 이벤트명을 인자로 받고, 그 이벤트명의 이벤트를 데이터 스트림으로 표현하는 Observable 객체를 반환합니다. 그래서 위 코드와 같이 Observable을 inputStream이라고 이름을 지었어요. 그럼 이제 이 데이터 스트림을 적절히 변경해서 API 호출을 하고, 응답을 받아 계산하고, 계산된 결과를 렌더링을 하는 작업을 해볼게요.

    const inputStream = fromEvent(searchInput, 'input')
      .pipe(
        ...
      )

fromEvent가 Observable을 반환한다고 했죠? Observable 객체에는 `pipe` 라는 함수가 있어요. 이 함수를 통해서 데이터 스트림이 어떻게 변화하는지를 표현할 거에요. 그리고 이 함수는 다시 새로운 Observable 객체를 반환해요. 그래서 최종적으로 inputStream에는 데이터 스트림이 어떻게 변화하는지를 모두 표현하고 있는 새로운 Observable 객체가 할당될 거에요. 이제 pipe 안에 데이터 스트림의 변화를 표현해봅시다.

    const inputStream = fromEvent(searchInput, 'input')
      .pipe(
        map((event) => event.target.value),
        debounceTime(1000),
        distinctUntilChanged(),
        tap(() => recipeList.innerHTML = '<h3>Loading...</h3>'),
        switchMap((inputValue) =>
          ajax(`${apiEndPoint}?f=${inputValue}`, { method: 'GET' })
            .pipe(
              map(({ response }) => response ? response.meals : []),
            ),
        ),
      )

여기서 사용된 함수들(RxJS에서 자주 사용되기도 하는)에 대해서 간단하게 소개를 해드릴게요. 

map은 이름 그대로 어떤 값을 받아서 다른 값으로 매핑을 해주는 역할을 해요. 즉, a라는 값을 받아서 계산을 한 뒤에 b라는 값을 반환해요.

debounceTime은 우리가 Vanilla JS로 만들었던 코드에서 lodash의 debounce와 같은 역할을 하는 함수에요. 주어진 밀리세컨드만큼 디바운싱을 합니다.

distinctUntilChanged는 우리가 이전 코드에서 previousInputValue로 이전 값과 새 값이 같으면 API 호출을 막았던 역할을 해요. map, debounceTime 을 거쳐 데이터가 도착했는데 이전 값과 같다면 다음 Operator로 넘어가지 않게 만들어요!

tap은 `가볍게 두드리다` 라는 뜻인데, 여기서도 동일한 의미로 사용돼요. 이 tap은 map과 달리 도착한 데이터에 어떠한 변경도 하지 않아요. 그냥 데이터가 tap에 도착하면 데이터에 영향을 주지 않는 어떤 작업을 수행하고 다음 Operator로 데이터가 넘어가요. 그래서 이 tap에서 log를 남기거나 할 수 있어요.

지금 우리는 searchInput이 내보내는 `input` 이벤트를 Observable로 표현하고 있어요. 그런데 우리의 목표는 이 input 값을 우리가 원하는 시점에 API 호출을 해야하죠? 앞서 말했듯이 API를 호출하고 그 응답을 받는 것도 하나의 데이터 스트림이에요. RxJS에서는 Observable 안에서 새로운 Observable을 표현할 수 있어요. 그러니까 [input tag의 값이 흘러서 API를 호출하는] 형태인거죠. 그런데 우리는 이런 중첩된 데이터 스트림의 구조에 상관없이 [input tag의 값이 흘러서 API를 호출하고 그 결과를 받고] 싶어요. 그래서 이렇게 바깥 Observable에서 내부 Observable의 결과를 사용할때 mergeMap이라는 Operator를 사용해요. 그런데 만약 apple pie로 검색한 API가 아직 도착하지 않았는데 peacan pie라는 값으로 새로 API를 호출하면 이전 API는 무시하거나 취소를 해야겠죠? mergeMap의 역할을 하면서 새 값이 들어오면 이전 작업을 취소해주는 Operator가 switchMap이에요!

ajax는 이름에서도 알 수 있듯이 API 요청을 하는 함수에요! 그리고 앞서 말했듯이 이 녀석이 API 데이터의 흐름을 표현하는 Observable을 반환해요.

자, 이제 input tag에서 오는 값을 받아 API를 호출하고, 응답 결과를 적절하게 가져오는 것까지 Observable과 Operator를 사용해 표현했어요. 이제 이 데이터 스트림에서 실제로 데이터를 가져와 렌더링을 해야합니다.

    inputStream.subscribe({
      ...
    })

이렇게 inputStream Observable 객체의 `subscribe` 함수를 호출하면 데이터를 받기 시작해요! 그런데 이 subscribe 함수는 next, error, complete 라는 함수 프로퍼티를 갖는 객체를 받거나 이 각각의 함수 세개를 연달아 받아요. next는 다음 값이 올때마다 호출이 되고, error는 이 데이터 스트림 과정에 에러가 있을때 호출이 되고, complete는 Observable의 구독이 완료되었을때 호출이 됩니다. 우리는 레시피를 next에서 받아 렌더링 할 것이기 때문에 next 함수를 작성해볼게요. 그리고 에러가 났을때도 적절하게 표현을 해봅시다.

    inputStream.subscribe({
      next: (meals) => {
        recipeList.innerHTML = meals
          .map((meal) => new Meal(meal))
          .map((mealInstance) => mealInstance.renderToString())
          .join('')
      },
      error: () => {
        recipeList.innerHTML = '<h3>An error has occurred when fetching data...</h3>'
      },
    })

next 함수 바디의 내용이 이전 코드와 동일하죠? 브라우저로 확인해보면 이전 코드와 동일하게 동작하는 것을 확인할 수 있어요.

간단한 코드를 RxJS를 이용해 다시 표현을 해보았는데, 애플리케이션에서 데이터가 어떻게 흐르고, 어떤 시점에 어떤 동작을 해야하는지 이해하고 있다면 RxJS를 사용해 데이터의 흐름을 관리하는 것도 좋을 것 같지 않나요? 이미 react + redux + redux-observable을 사용하고 계신 분들은 더 체감하실 수도 있을 것 같네요! 이상, DevFest WebTech 2019 CodeLab 이었습니다!

### 참고하면 좋은 문서

- [https://en.wikipedia.org/wiki/ECMAScript](https://en.wikipedia.org/wiki/ECMAScript)
- [https://en.wikipedia.org/wiki/Ecma_International](https://en.wikipedia.org/wiki/Ecma_International)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_2015_support_in_Mozilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_2015_support_in_Mozilla)
- [http://www.ecma-international.org/ecma-262/6.0/](http://www.ecma-international.org/ecma-262/6.0/)
- [https://developer.mozilla.org/en-US/docs/Archive/Web/JavaScript/ECMAScript_Next_support_in_Mozilla](https://developer.mozilla.org/en-US/docs/Archive/Web/JavaScript/ECMAScript_Next_support_in_Mozilla)
- [https://rxjs-dev.firebaseapp.com/guide/observable](https://rxjs-dev.firebaseapp.com/guide/observable)
- [http://sculove.github.io/blog/2017/10/07/rxjsbook4/](http://sculove.github.io/blog/2017/10/07/rxjsbook4/)

### Reference

<https://www.notion.so/DevFest-WebTech-CodeLab-fcc4ab44f4e34efe850a199dcb95ad01>
