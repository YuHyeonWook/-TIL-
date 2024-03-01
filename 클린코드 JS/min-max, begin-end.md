# min-max, begin-end

# min-max (경계 다루기)

- 경계 다루기
- min-max에 포함되는지 포함 안되는지에 대한 경계를 알아보자.

```jsx
function genRandomNumber(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min; // ⓐ
}

// 상수
const MIN_NUMBER = 1;
const MAX_NUMBER = 45;

genRandomNumber(MIN_NUMBER, MAX_NUMBER); // ⓑ
```

- genRandomNumber()함수는 난수를 생성하는 함수이다.
- genRandomNumber() 함수에 대한 상수를 만들어 놓으면 남들이 봤을 때 무슨 역할을 하는지 명시적으로 알 수 있게 된다.
- ⓐ 함수의 구현(함수의 내부)를 안보더라도, 명시적으로 표현된 ⓑ 함수 호출의 인수를 보고 예상할 수 있다.

```jsx
const MAX_AGE = 20;

// 성인의 여부를 판별하는 함수
function isAdult(age) {
	if (age >= 20) {
		return;
}
```

- 위 코드는 성인 여부를 판별하는 함수이다.
- 회사 팀내에서 min과 max에 대한 컨벤션을 정해야한다.
  - ex) max과 max를 다룰때 최솟값, 최댓값을 포함되는지 안되는지, 이상 인지 초과인지 , 이하인지 미만인지 에 대해 규칙을 정해야 헷갈리지 않는다.

```jsx
// 상수
const MIN_NUMBER_LIMIT = 1;
const MAX_NUMBER_LIMIT = 45;
```

- 이렇게 상수 네이밍에 LIMIT을 정하여 1과 45를 넘으면 안된다고 명시적으로 표현할 수 있다.

### 정리

경계를 다룰 때에는

1. 최솟값과 최댓값을 다루어야한다.
2. 최솟값과 최댓값의 포함여부를 결정해야한다.(이상- 초과 / 이하-미만) 혹은 네이밍에 최솟값과 최댓값의 포함 여부를 명시한다.

<aside>
💡 즉, 명시적인 코드를 작성하도록 노력해라!!! 그러면 다른 사람이 볼때 직관적으로 볼 수 있어서 편해진다.

</aside>

# begin-end(경계를 포함하지만 제외하는 경우)

- 경계를 포함하지만 제외하는 경우
- 경계를 포함할 수 있지만, 경계를 제외하는 경우도 있다. 이는 시작과 끝이 동일하지 않다는 것이다.
- ex) 숙소 체크인 날짜를 시작과 끝의 예시로 표현할 수 있다. 체크인-begin, 체크아웃-end 이다.

```jsx
// 경계를 포함하지만 제외하는 경우
function reservationDate(beginDate, endDate) {
  // ....some code
  return;
}

reservationDate("YYYY-MM-DD", "YYYY-MM-DD");
```

- 함수명을 한눈에 이해할 수 있게 직관적으로 지으면 **시작과 끝**이라고 이해할 수 있게 된다.
