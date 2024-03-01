# async & await

### Promise then() 지옥

- then() 메서드가 지나치게 체인되어 반복되면 코드가 장황해지고 가독성이 굉장히 떨어질 수 가 있다.

```jsx
fetch("https://api.github.com/users")
  .then((response) => {
    if (response.ok) {
      return response.json();
    } else {
      throw new Error("Network Error");
    }
  })
  .then((users) => {
    return users.map((user) => user.login);
  })
  .then((logins) => {
    return logins.join(", ");
  })
  .then((result) => {
    console.log(result);
  })
  .catch((error) => {
    console.error(error);
  });
```

- 위 예시 코드는 fetch 함수를 사용하여 깃허브 API에서 유저 정보를 가져오고, then 메서드를 여러 번 연결하여 유저들의 로그인 이름을 쉼표로 구분한 문자열로 만들어 출력하는 비동기 작업을 수행한다.
- 이런식으로 then을 늘어뜨어 놓으면 코드가 길어지고, 각 then 메서드가 어떤 값을 반환하는지 파악하기 어렵게 된다.
- 또한, catch 메서드가 마지막에 한 번만 사용되어 있기 때문에, 중간에 발생할 수 있는 에러나 예외 상황에 대응하기 어렵다.
- 이를 극복하기 위해 나온 것이 async/await키워드이다.

```jsx
try {
  const response = await fetch("");
  if (response.ok) {
    const users = await response.json();
    const logins = users.map((user) => user.login);
    const result = logins.join(", ");
    console.log(result);
  } else {
    throw new Error("Network Error");
  }
} catch (error) {
  console.error(error);
}
```

- async/await 키워드는 ES8에서 도입된 비동기 처리를 위한 문법으로, 프로미스를 기반으로 하지만 then과 catch 메서드를 사용하지 않고 비동기 작업을 수행할 수 있다. async/await 키워드를 사용하면 비동기 작업을 마치 동기 작업처럼 쓸 수 있어서 코드가 간결하고 가독성이 좋아지게 된다.

# async & await

- async/await는 ES2017에 도입된 문법으로서, Promise 로직을 더 쉽고 간결하게 사용할 수 있게 해준다.
  - 유의해야 할 점이 async/await가 Promise를 대체하기 위한 기능이 아니다.
  - 내부적으로는 여전히 Promise를 사용해서 비동기를 처리하고, 단지 코드 작성 부분을 프로그래머가 유지보수하게 편하게 보이는 문법만 다르게 해줄 뿐이라는 것이다.

## async / await 기본 사용법

- async 와 await 는 절차적 언어에서 작성하는 코드와 같이 사용법도 간단하고 이해하기도 쉽다.
  - `await` 키워드는 오직 `async` 로 정의된 함수의 내부에서만 사용될 수 있다.
  - function 키워드 앞에async 만 붙여주면 되고, 비동기로 처리되는 부분 앞에 await 만 붙여주면 된다.
  - 모든 `async` 함수는 암묵적으로 promise를 반환하고, promise가 함수로부터 반환할 값을 resolve 한다.

```jsx
// 프로미스 객체 반환 함수
function delay(ms) {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log(`${ms} 밀리초가 지났습니다.`);
      resolve();
    }, ms);
  });
}
```

```jsx
// 기존 Promise.then() 형식
function main() {
  delay(1000)
    .then(() => {
      return delay(2000);
    })
    .then(() => {
      return Promise.resolve("끝");
    })
    .then((result) => {
      console.log(result);
    });
}

// 메인 함수 호출
main();
```

- 위 코드에서 promise는 then 메서드를 연속적으로 사용하여 비동기 처리를 하지만, async/await는 await 키워드로 비동기 처리를 기다리고 있다는 것을 직관적으로 표현하고 있음을 볼 수 있다.

```jsx
// async/await 방식
async function main() {
  await delay(1000);
  await delay(2000);
  const result = await Promise.resolve("끝");
  console.log(result);
}

// 메인 함수 호출
main();
```

- async/await의 장점은 비동기적 접근방식을 동기적으로 작성할 수 있게 해주어 코드가 간결해지며 가독성을 높여져 유지보수를 용이하게 해준다.

## async 키워드

- function 앞에 async을 붙여줌으로써, 함수 내에서 await 키워드를 사용할 수 있게 된다. 이는 반대로 말하면 await 키워드를 사용하기 위해선 반드시 async function 정의가 되어 있어야 한다는 말과 같다.

```jsx
// 함수 선언식
async function func1() {
  const res = await fetch(url); // 요청을 기다림
  const data = await res.json(); // 응답을 JSON으로 파싱
}
func1();

// 함수 표현식
const func2 = async () => {
  const res = await fetch(url); // 요청을 기다림
  const data = await res.json(); // 응답을 JSON으로 파싱
};
func2();
```

```jsx
function hello() {
  return "hello";
}

async function helloAs() {
  return "hello Asy";
}

// 결과
hello(); //'hello'
helloAs(); // Promise {<fulfilled>: 'hello Asy'}
```

- 위를 출력하면 Promise 객체가 출력됨
- 함수 앞에 **async**를 붙이면 자동적으로 **Promise를 리턴하는 비동기 처리 함수**가 된다.

```jsx
function hello() {
  return "hello";
}

async function helloAs() {
  return "hello Asy";
}
helloAs().then((res) => {
  console.log(res);
});

// 결과
// hello Asy
```

### async 리턴값은 Promise 객체

- async 키워드를 붙인 function에서 값을 리턴한 결과는?

```jsx
async function func1() {
  return 1;
}

const data = func1();
console.log(data); // 프로미스 객체가 반환된다
```

![image](https://github.com/YuHyeonWook/TIL/assets/110236953/1ea2fc32-ddbc-4291-b26e-670dab3301aa)

- 정수 1을 리턴했음에도 위 결과에서 보듯이, 이행(fulfilled) 상태의 프로미스 객체 형태로 반환됨을 볼 수 있다. 이를 통해 **async function에서 어떤 값을 리턴하든 무조건 프로미스 객체로 감싸져 반환 된다**는 특징을 알 수 있다.

### 다른 Promise 상태를 반환하기

- 직접 프로미스 정적 메서드를 통해 다음과 같이 프로미스 상태(state)를 다르게 지정하여 반환이 가능하다.

```jsx
async function resolveP() {
  return Promise.resolve(2);
}

async function rejectP() {
  return Promise.reject(2);
}
```

![image](https://github.com/YuHyeonWook/TIL/assets/110236953/0a95722f-4d15-44ab-aef1-6ae13a98abb9)

- reject 같은 경우 위와 같이 Promise.reject() 정적 메서드를 통해 반환되는 프로미스 상태를 실패(rejected) 상태로 지정해줄 수 있지만, async 함수 내부에서 예외 throw를 해도 실패 상태의 프로미스 객체가 반환되게 된다.

```jsx
async function errorFunc() {
  throw new Error("프로미스 reject 발생시킴");
}
```

![image](https://github.com/YuHyeonWook/TIL/assets/110236953/34436213-5bf0-484e-885e-f8e29e789453)

- async function에서 일부러 return을 하지 않아도 자동으로 return undefiend 으로 처리 되기 때문에 무조건 프로미스 객체를 반환하게 된다.

![image](https://github.com/YuHyeonWook/TIL/assets/110236953/f265545a-0734-463e-ba1d-a10daa10a6a3)

### async 함수와 then 핸들러

- async 함수의 리턴값은 프로미스 객체이기 때문에 async 함수 자체에 then 핸들러를 붙일 수도 있다.

```jsx
async function func1() {
  return 1;
}

func1().then((data) => console.log(data));
```

## await 키워드

- await는 promise.then() 보다 깔끔한 코드(?)로 비동기 처리의 결과값을 얻을 수 있도록 해주는 문법이다.
- 예를들어 서버에 리소스를 요청하는 fetch() 비동기 함수를 then 핸들러 방식으로 결과를 얻어 사용해왔을 것이다.

```jsx
// then 핸들러 방식
fetch(url)
  .then((res) => res.json()) // 응답을 JSON으로 파싱
  .then((data) => {
    // data 처리
    console.log(data);
  });
```

- await 키워드를 사용하면 then 핸들러를 복잡하게 처리할 필요 없이, 심플하게 비동기 함수 왼쪽에 await만 명시해주고 결과값을 변수에 받도록 코드를 정의하면 끝이다.
- then과 콜백 함수를 남발하여 코드가 들여쓰기로 깊어지는 것을 방지하고, 한 줄 레벨에서 코드를 나열하여 가독성을 높일 수 있다.

```jsx
// await 방식
async function func() {
  const res = await fetch(url); // 요청을 기다림
  const data = await res.json(); // 응답을 JSON으로 파싱
  // data 처리
  console.log(data);
}
func();
```

### await는 Promise 처리가 끝날때까지 기다림

- **await은 Promise 비동기 처리가 완료될때 까지 코드 실행을 일시 중지하고 wait 한다**라는 뜻이다.
  - 예를 들어 fetch() 함수를 사용하여 서버에서 데이터를 가져오는 경우를 떠올려보면, 이 fetch() 함수는 Promise를 반환한다. 따라서 await 키워드를 사용하여 Promise가 처리될 때까지 코드 실행을 일시 중지하고, Promise가 처리되면 결과 값을 반환하여 변수에 할당하는 식이다.

```jsx
async function getData() {
  const response = await fetch('https://jsonplaceholder.typicode.com/users/1');
  const data = await response.json();
  console.log(data):
}
```

- 위 예제를 보면 getData() async 함수 내에서 fetch() 비동기 함수를 호출하고, 반환된 Promise를 await 키워드로 처리한다.
- 이로 인해 함수 내 코드 실행이 일시 중지되고, fetch() 함수가 완료될 때까지 기다리게 된다. → 서버로부터 리소스를 성공적으로 가져와 fetch() 함수가 완료되면, 바로 다음response.json() 함수를 호출하여 반환된 Promise를 다시 await로 처리한다. → 다시 데이터를 가져오는 동안 코드 실행이 일시 중지되고, 데이터가 성공적으로 가져와지면 최종 결과 값을 반환한다.
- 따라서 **await는 Promise를 처리하고 결과를 반환하는 데, 비동기적인 작업을 동기적으로 처리할 수 있게 되는 것**이다.

### async/await 에러 처리

- 기존의 Promise.then() 방식의 에러 처리는 catch() 핸들러를 중간 중간에 명시함으로써 에러를 받아야만 했고, 일반적으로 에러를 처리하기 위해선 try/catch 문을 사용하여 에러를 처리해왔다 .

```jsx
// then 핸들러 방식
function fetchResource(url) {
  fetch(url)
    .then((res) => res.json()) // 응답을 JSON으로 파싱
    .then((data) => {
      // data 처리
      console.log(data);
    })
    .catch((err) => {
      // 에러 처리
      console.error(err);
    });
}
```

- async/await도 비동기 처리에 대한 에러를 처리할 필요가 생기면 그대로 try/catch문을 씌우면 된다.

```jsx
// async/await 방식
async function func() {
  try {
    const res = await fetch(url); // 요청을 기다림
    const data = await res.json(); // 응답을 JSON으로 파싱
    // data 처리
    console.log(data);
  } catch (err) {
    // 에러 처리
    console.error(err);
  }
}
func();
```

- 이처럼 async/await의 장점은 비동기 코드를 마치 동기 코드처럼 읽히게 해준다는 것이다. 우리가 일반적으로 코드를 쓰고 읽어 내리듯이

## 적절한 async/await 사용

- await 키워드를 사용하면 비동기가 강제적으로 동기 처리가 되어 코드가 순차적으로 수행된다. 그러면 이를 어떻게 ‘병렬 처리’ 한다는 것일까? 핵심은 프로미스 객체 함수를 await과 같이 써서 실행시키는 게 아니라, 미리 함수를 동기/논블록킹으로 실행하고 그 결과 프로미스 값을 await를 통해 받는 식이다.
- 기존에는 비동기 처리 요청을 하고 동시에 요청이 완료될때 까지 await 하였기 때문에 1초안에 처리될 것이 2초가 걸렸다.

```jsx
async function getFruites() {
  let a = await getApple(); // getApple() 비동기 처리를 요청하고, 요청이 처리될때 까지 기다림 (1초 소요)
  let b = await getBanana(); // getBanana() 비동기 처리를 요청하고, 요청이 처리될때 까지 기다림 (1초 소요)
  console.log(`${a} and ${b}`); // 총 2초 소요
}
```

- getApple() 와getBanana() 비동기 로직이 순서를 지켜야하는 로직이라면 위와 같이 구성하여야 하는 것이 옳지만, 현재로서는 서로 연관 없기 때문에 반드시 순차적으로 실행 시킬 필요가 없다. 따라서 비동기 처리 요청과 값을 await 하는 로직을 분리시키면 된다.

```jsx
async function getFruites(){

  let getApplePromise = getApple(); // async함수를 미리 논블록킹으로 실행한다.
  let getBananaPromise = getBanana(); // async함수를 미리 논블록킹으로 실행한다.

  // 이렇게 하면 각각 백단에서 독립적으로 거의 동시에 실행되게 된다.
  console.log(getApplePromise)
  console.log(getBananaPromise)

  let a = await getApplePromise; // 위에서 받은 프로미스객체 결과 변수를 await을 통해 꺼낸다.
  let b = await getBananaPromise; // 위에서 받은 프로미스객체 결과 변수를 await을 통해 꺼낸다.

  console.log(`${a} and ${b}`); // 본래라면 1초+1초 를 기다려야 하는데, 위에서 1초기다리는 함수를 바로 연속으로 비동기로 불려왔기 때문에, 대충 1.01초만 기다리면 처리된다.
})
```

![image](https://github.com/YuHyeonWook/TIL/assets/110236953/94f4053a-a1e5-4674-802e-535f5dc4f04a)

## Async/Await 내부 동작 과정

```
const one = () => Promise.resolve('One!');

async function myFunc(){
	console.log('In function!');
	const res = await One();
	console.log(res);
}

console.log('Before Function!');
myFunc();
console.log('After Function!');
```

![image](https://github.com/YuHyeonWook/TIL/assets/110236953/65add181-dadd-462c-b590-456ba24d2c94)

1. 콘솔에 'Before Function!' 이 출력된다.
2. async 함수인myFunc() 이 호출된다.
3. async 함수 안에 있는 콘솔 함수가 실행되어 콘솔에 'In Function!' 이 출력된다.
4. Promise 객체를 반환하는one() 비동기 함수를 호출한다.
5. 이때one() 비동기 함수 왼쪽에await 키워드로 인해,myFunc 함수의 내부 실행은 잠시 중단되고 Call stack 에서 빠져나와 나머지 부분은 Microtask Queue 에 적재된다. 이는 자바스크립트 엔진이await 키워드를 인식하면 async 함수의 실행은 지연되는 것으로 처리하기 때문이다.
6. 마지막으로 콘솔에 'After Function!' 이 출력된다.
7. 모든 메인 스레드의 자바스크립트 코드가 실행이되어 더이상 Call Stack엔 실행할 스택이 없어 비워지게 된다.
8. 그러면 이벤트 핸들러가 이를 감지하여, Microtask Queue에 남아있는 async 함수를 빼와 Call Stack에 적재하게 된다.
9. Promise 객체의 결과물인 'One!' 문자열을 변수res 에 받고 이를 콘솔에 출력한다.

<br>

# 참고

- [https://inpa.tistory.com/entry/🔄-자바스크립트-이벤트-루프-구조-동작-원리](https://inpa.tistory.com/entry/%F0%9F%94%84-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84-%EA%B5%AC%EC%A1%B0-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC)
- [https://medium.com/@constell99/자바스크립트의-async-await-가-promises를-사라지게-만들-수-있는-6가지-이유-c5fe0add656c](https://medium.com/@constell99/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%9D%98-async-await-%EA%B0%80-promises%EB%A5%BC-%EC%82%AC%EB%9D%BC%EC%A7%80%EA%B2%8C-%EB%A7%8C%EB%93%A4-%EC%88%98-%EC%9E%88%EB%8A%94-6%EA%B0%80%EC%A7%80-%EC%9D%B4%EC%9C%A0-c5fe0add656c)
- [https://inpa.tistory.com/entry/JS-📚-비동기처리-async-await](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EB%B9%84%EB%8F%99%EA%B8%B0%EC%B2%98%EB%A6%AC-async-await)
