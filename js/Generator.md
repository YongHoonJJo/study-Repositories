### < Generator >

#### Generator 란.

- 함수의 실행을 중간에 멈추고 재개할 수 있는 기능
- 실행을 멈출 때 값을 전달할 수 있기 때문에 반복문에서 제너레이터가 전달하는 값을 하나씩 꺼내서 사용할 수 있다.
- 필요한 순간에만 값을 계산해서 전달할 수 있기 때문에 메모리 측면에서 효율적.
- 그 외 다른 함수와 협업 멀티태스킹이 가능.

<br>

#### 제너레이터 이해하기

```js
function *gen() {
  yield 10;
  yield 20;
  return 'finished';
}
```

- 제너레이터는 일반함수 이름 앞에 *을 붙여서 제너레이터 함수를 만든다.
- `yield` 키워드를 통해 함수의 실행을 멈출 수 있다. 
- 제너레이터는 제너레이터 객체를 리턴하는 함수이다. 



제너레이터 객체는 `next`, `return`, `throw` 메서드를 가진다.

**next()**

```js
function *f() {
  console.log('f-1')
  yield 10
  console.log('f-2')
  yield 20
  console.log('f-3')
  return 'finished'
}

const gen = f() 
console.log(gen.next())
// f-1
// { value: 10, done: false }
console.log(gen.next())
// f-2
// { value: 20, done: false }
console.log(gen.next())
// f-3
// { value: 'finished', done: true }
console.log(gen.next())
// { value: undefined, done: true }
```

- 제너레이터 함수를 호출시 함수 내부가 실행되지 않는다.
- `next()` 메서드를 호출하면 yield 키워드를 만날 때 까지 실행하고, 객체를 리턴한다. 

<br>

**return()**

```js
function *f() {
  console.log('f-1')
  yield 10
  console.log('f-2')
  yield 20
  console.log('f-3')
  return 'finished'
}

const gen2 = f()
console.log(gen2.next())
// f-1
// { value: 10, done: false }
console.log(gen2.return('react'))
// { value: 'react', done: true }
console.log(gen2.next())
// { value: undefined, done: true }
console.log(gen2.next())
// { value: undefined, done: true }
```

- `return()` 메서드를 호출 시 전달한 값이 포함된 객체가 리턴되며, done 은 true 상태이다. 이후 next() 를 호출해도  value 의 값은 undefined 이다.

<br>

**throw()**

```js
const gen3 = f2()
console.log(gen3.next())
// f2-1
// { value: 10, done: false }
console.log(gen.throw('gen throww'))
// gen throww
```

- `throw()` 메서드를 호출하면 catch 문으로 들어간다고 하는데,,,
- 책의 내용은 <https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Generator/throw> 과 동일.

<br>

#### 이터레이터 (iterator) : 반복자

- next() 메서드를 가지고 있다.
- next() 메서드는 value 와 done 프로퍼티를 가진 객체를 리턴한다.
- done 속성값은 작업이 끝났을 때 true 가 된다. 
- 이터러블이다. (well-formed iterator)

<br>

####이터러블 (utterable)

- `Symbol.iterator` 를 속성값으로 함수를 가지고 있고, 이 함수는 이터레이터를 리턴한다.

- 이터레이터는 자기 자신을 리턴하는 `Symbol.iterator()` 메서드를 가지고 있다.

- ```js
  function *f() {
    console.log('f-1')
    yield 10
    console.log('f-2')
    yield 20
    console.log('f-3')
    return 'finished'
  }
  
  const gen = f()
  console.log(gen === gen[Symbol.iterator]())
  ```

<br>

**커스텀 이터러블**

```js
const iterable = {
  [Symbol.iterator]() {
    let i = 3;
    return {
      next() {
        return i==0 ? { done: true } : { value: i--, done: false }
      },
      [Symbol.iterator]() { return this; }
    }
  }
}

let iterator = iterable[Symbol.iterator]();
for(const a of iterator) 
  console.log(a);
```

- `[Symbol.iterator]() { return this; }` 이 코드가 추가되지 않으면, 위의 코드에서는 iterator 가 iterable 이 아니라는 오류가 발생한다

<br>

#### 전개 연산자 (Spread Operator)

전개연산자는 이터러블이어야 가능하다.

```js
function *f() {
  console.log('f-1')
  yield 10
  console.log('f-2')
  yield 20
  console.log('f-3')
  return 'finished'
}

const gen = f()
console.log(...gen)
// f-1
// f-2
// f-3
// 10 20
```

<br>

```js
const a = [10, 20, 30]
const it = a[Symbol.iterator]()

console.log(it.next())
// { value: 10, done: false }

console.log(...a)
// 10 20 30
```

- 배열은 이터레이블이다. 

<br>

```js
const a = [10, 20, 30]
const it = a[Symbol.iterator]()

console.log(it.next())
// { value: 10, done: false }

console.log(...a)
// 10 20 30

console.log(...it)
// 10 20 30
```

<br>

```js
const a = [10, 20, 30]
a[Symbol.iterator] = null
console.log(...a)
```

- TypeError: Found non-callable @@iterator 라는 에러 발생.

<br>

#### for ... of 문과 이터러블과의 관계

**ES6 이전에 리스트 순회**

```js
const list = [1, 2, 3];
for(var i=0; i<list.length; i++) {
    console.log(list[i]);
}

const str = 'abc';
for(var i=0; i<str.length; i++) {
    console.log(str[i]);
}
```

<br>

**ES6 에서의 리스트 순회**

```js
const list = [1, 2, 3];
for(const a of list) {
    console.log(a);
}

const str = 'abc';
for(const s of str) {
    console.log(s);
}
```

- 여기까지만 보면 `for...of` 문은 인덱스를 활용하는 것 처럼 보일 수 있다.

<br>

```js
const set = new Set([1, 2, 3, 1]);
for(const a of set) console.log(a);
// 1
// 2
// 3

const map = new Map([['a', 1], ['b', 2], ['c', 3]]);
for(const a of map) console.log(a);
// [ 'a', 1 ]
// [ 'b', 2 ]
// [ 'c', 3 ]
```

<br>

**Map 과 Set 의 객체에도 인덱스를 통한 접근이 가능할까?**

```js
const set = new Set([1, 2, 3, 1]);
console.log(set[0]) 

const map = new Map([['a', 1], ['b', 2], ['c', 3]]);
console.log(map[0])
```

- 모두 undefined 가 출력된다.

<br>

**Map 과 Set 이 for ... of 문에서 작동할 수 있는 이유는..?**

```js
const set = new Set([1, 2, 3, 1]);
console.log(set[Symbol.iterator]())
// [Set Iterator] { 1, 2, 3 }

set[Symbol.iterator] = null
for(const a of set) console.log(a);
// TypeError: set is not iterable
```

- Map 의 경우도 마찬가지 결과를 확인할 수 있다.

<br>

**전개연산자**

```js
const set = new Set([1, 2, 3, 1]);
console.log(...set)
// 1 2 3

set[Symbol.iterator] = null
console.log(...set)
// TypeError: Found non-callable @@iterator
```

<br>

####  제너레이터로 구현한 map, filter

```js
const map = (f, iter) => {
  for(const a of iter)
    yield f(a)
}

const filter = (f, iter) => {
  for(const a of iter)
    if(f(a)) yield a
}
```

- 위의 map 과 filter 는 Array 뿐만 아니라, 모든 이터러블에 대해 적용이 가능하다.
- 지연적으로 원하는 상황에서만 `yield` 가 진행되어, 그때만 해당하는 값을 얻을 수 있다
- 즉, map, filter, reduce 등을 조합하여 사용할 때, 배열을 모두 만들어놓고 연산을 할 필요가 없어지게 된다.

<br>

**Ex) QueryString**

```js
const map = function *(f, iter) {
  for(const a of iter) yield f(a);
}

const reduce = (f, acc, iter) => {
  if(!iter) {
      iter = acc[Symbol.iterator]();
      acc = iter.next().value;
  }
  for(const a of iter)
      acc = f(acc, a);
  return acc;
}

const join = (sep = ',', iter) => reduce((a, b) => `${a}${sep}${b}`, iter)

const qs = (obj) => join('&', map(([k, v]) => `${k}=${v}`, Object.entries(obj)))

console.log(qs({ limit : 10, offset : 10, type : 'notice'}))
```

<br>

#### 효율성 비교

```js
const add = (a, b) => a + b;

const range = l => {
    let i = -1;
    let res = [];
    while (++i < l) {
        res.push(i);
    }
    return res;
};

let list;

console.time('range');
list = range(10000000);
log(reduce(add, list));
console.timeEnd('range');

const L = {};
L.range = function *(l) {
    let i = -1;
    while (++i < l) {
        yield i;
    }
};

console.time('L.range');
list = L.range(10000000);
log(reduce(add, list));
console.timeEnd('L.range')
```

- 실행 결과는..?
- `range(100000000)` vs `L.range(100000000)` 의 결과는..?

<br>

#### yield*

```js
function *g1() {
  yield 2;
  yield 3;
}

function *g2() {
  yield *g1()
}

function *g3() {
	for(const a of g1()) yield a
}
```

- `yield *iterable`은 `for (const val of iterable) yield val;` 과 같다.

<br>

#### 제너레이터 함수로 데이터 전달하기

```js
function *f() {
  const data1 = yield 10
  console.log(data1)
  const data2 = yield 20
  console.log(data2)
}

const gen = f()
console.log(gen.next())
// { value: 10, done: false }
console.log(gen.next(11))
// 11
// { value: 20, done: false }
console.log(gen.next(22))
// 22
// { value: undefined, done: true }
```

<br>

#### 협업 멀티태스킹

제너레이터는 다른 함수와 협업 멀티태스킹을 할 수 있는데, 멀티태스킹이란 여러 개의 태스크를 실행할 때 하나의 태스크가 종료되기 전에 멈추고 다른 태스크가 실행되는 것을 의미한다. 

**협업 멀티태스킹 방식으로 동작하는 코드의 예시**

```js
function *minsu() {
  const myMsgList = [
    '안녕 나는 민수야',
    '만나서 반가워',
    '내일 영화 볼래?',
    '시간 안 되니?',
    '내일 모레는 어때?'
  ]
  for(const msg of myMgsList)
    console.log('수지: ', yield msg)
}

function suji() {
  const myMsgList = [
    '',
    '안녕 나는 수지야',
    '그래 반가워',
    '...'
  ]
  const gen = minsu()
  for(const msg of myMsgList)
    console.log('민수: ', gen.next(msg).value)
}

suji()
```

