# 재귀(Recursion)

자기 자신을 호출하는 절차

## 📚 Call Stack

- 함수를 호출하며 호출 스낵의 꼭대기에 쌓임
- 자바스크립트의 보이지 않는 곳에서 작동하는 정적 데이터 구조

```javascript
function takeShower() {
  return 'Showering!';
}

function eatBreakfast() {
  let meal = cookFood();
  return `Eating ${meal}`;
}

function cookFood() {
  let items = ['Oatmeal', 'Eggs', 'Protein Shake'];
  return items[Math.floor(Math.random() * items.length)];
}
function wakeUp() {
  takeShower();
  eatBreakfast();
  console.log('Ok ready to go to work!');
}

wakeUp();
```

1. wakeUp() 호출
2. takeShower()가 호출 스택에 추가 => "Showering!" 반환
3. 값을 반환했기 때문에 호출 스택에서 제거
4. 다시 wakeUp()으로 돌아가서 eatBreakfast() 호출 후 스택에 추가
5. eatBreakfast()에서 cookFood() 호출
6. items 배열에서 무작위 요소 반환
7. 다시 eatBreakfast()로 돌아가서 `Eating ${meal}`반환
8. eatBreakfast() 스택에서 제거
9. wakeUp()으로 돌아간 후 콘솔로그 호출

✔️ 함수를 호출하면 스택의 꼭대기에 추가되고, 값이 반환되면 꼭대기에서 하나씩 제거된다.

## 📚 재귀함수

재귀함수의 두 가지 기본 요소

1. Base Case : 종료 조건은 재귀가 멈추는 시점
2. 다른 입력값 : 걑은 함수를 계속 호출하지만, 다른 입력값으로 함수를 호출해야 한다.

### Example 1

```javascript
// 재귀
function countDown(num) {
  // 종료조건(base case)
  if (num <= 0) {
    console.log('All done!');
    return;
  }
  console.log(num);
  num--;
  countDown(num);
}
countDown(3);

//print 3
//countDown(2);
//print 2
//countDown(1);
//print 1
//countDown(0);
//All done!

// 반복문
function countDown(num) {
  for (var i = num; i > 0; i--) {
    console.log(i);
  }
  console.log('All done!');
}
countDown(3);
```

### Example 2

```javascript
function sumRange(num) {
  if (num === 1) return 1; //Base case
  return num + sumRange(num - 1);
}

sumRange(4);
```

1. sumRange(num) 호출 시, num이 1일 때까지 num과 sumRange(num - 1)의 결과를 더하는 과정을 반복
2. base case에 도달하면 호출 스택은 거꾸로 반환

### Example 3

```javascript
// 반복문
function factorial(num) {
  let total = 1;
  for (let i = num; i > 1; i--) {
    total *= i;
  }
  return total;
}

// 재귀
function factorial(num) {
  if (num === 1) return 1; // base case
  return num * factorial(num - 1);
}
factorial(3);
// 3 * factorial(2)
// 2 * factorial(1)
// 1
```

## 💣 문제점

- 종료 조건이 없는 경우
- 잘못된 값을 반환하거나 반환하는 것을 잊은 경우
  => Stack overflow(재귀가 종료되지 않음)

## 📚 배열에서 모든 홀수값들을 찾는 함수

### Example 1. Helper Method Recursion

```javascript
function collectOddValues(arr) {
  // 초기화: 함수가 호출되면, 홀수 값을 저장할 빈 배열 result를 생성
  // 함수를 새로 호출할 때마다 배열이 리셋됨(함수의 지역 범위에 있기 때문에)
  let result = [];

  // 재귀 함수
  // 처음에는 전체 배열, 재귀가 진행될때 배열의 요소들이 하나씩 줄어듦
  // 최종적으로 빈배열이 되면 재귀 호출 종료
  function helper(helperInput) {
    // 종료 조건
    if (helperInput.length === 0) {
      return;
    }
    // 주어진 배열의 각 요소를 순회하면서 홀수인지 확인
    // 홀수인 경우, 이 값을 result 배열에 추가
    if (helperInput[0] % 2 !== 0) {
      result.push(helperInput[0]);
    }

    helper(helperInput.slice(1));
  }

  helper(arr);

  return result;
}

collectOddValues([1, 2, 3, 4, 5, 6, 7, 8, 9]);
```

### Example 2. Pure Method Recursion

중첩된 함수가 아닌 순수 재귀를 사용해서 문제를 해결할 수 있다.

```javascript
function collectOddValues(arr) {
  // 호출할때마다 리셋
  let newArr = [];

  if (arr.length === 0) {
    return newArr;
  }

  if (arr[0] % 2 !== 0) {
    newArr.push(arr[0]);
  }
  newArr = newArr.concat(collectOddValues(arr.slice(1)));
  return newArr;
}

collectOddValues([1, 2, 3, 4, 5]);
```

1. 첫 번째 호출: collectOddValues([1, 2, 3, 4, 5])

- newArr는 빈 배열 []로 시작합니다.
- 첫 번째 요소 1은 홀수이므로, newArr는 [1]이 됩니다.
- 함수는 나머지 배열 [2, 3, 4, 5]에 대해 자신을 재귀적으로 호출합니다.

2. 두 번째 호출: collectOddValues([2, 3, 4, 5])

- 이 호출에서 newArr는 다시 빈 배열 []로 시작합니다.
- 첫 번째 요소 2는 짝수이므로, newArr는 여전히 빈 배열입니다.
- 함수는 [3, 4, 5]에 대해 재귀적으로 호출합니다.

.
.
.

6. 여섯 번째 호출: collectOddValues([])

- 이 호출에서는 배열이 비어 있습니다. 따라서 기본 조건이 충족되고, 빈 배열 newArr를 반환합니다.

이제 함수는 각 재귀 호출에서 반환된 배열들을 역순으로 병합하기 시작합니다:

- [5]는 [4, 5] 호출로 돌아가며, [5]와 빈 배열을 병합하여 여전히 [5]가 됩니다.
- [5]는 [3, 4, 5] 호출로 돌아가며, [3]와 [5]를 병합하여 [3, 5]가 됩니다.
- [3, 5]는 [2, 3, 4, 5] 호출로 돌아가며, 빈 배열과 병합하여 여전히 [3, 5]가 됩니다.
- 마지막으로, [3, 5]는 원래 [1, 2, 3, 4, 5] 호출로 돌아가며, [1]과 병합하여 최종 결과 [1, 3, 5]를 생성합니다.

결과적으로, collectOddValues([1, 2, 3, 4, 5])의 최종 반환 값은 [1, 3, 5]가 됩니다.

```
배열을 사용하고 헬퍼 메소드 없이 순수 재귀를 통해 작성하는 경우, 배열을 복사하는 slice, spread 연산자, concat 같은 메소드를 사용할 수 있기 때문에 배열을 변경할 필요가 없다.
```
