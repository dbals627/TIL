# Big O란?

- 알고리즘의 성능을 나타내는 표기법
- 입력된 내용이 늘어날 수록 알고리즘 실행 시간이 어떻게 변하는지 설명하는 방식
- "데이터 원소가 N개일 때 알고리즘에 몇 단계가 필요할까?"
- Big-O로 측정되는 복잡성에는 시간 복잡도와 공간 복잡도가 있음
- N값의 증가에 따른 처리 시간 또는 필요 공간 계산

## 시간 복잡도

입력되는 n의 크기에 따라 실행되는 조작의 수

</br>
📈 빅오 복잡도 차트
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbvSKij%2FbtrCmflIZCF%2FgRGJGS5rNLxNqYxZMCIk8K%2Fimg.png">

### O(1)

Constant Time(상수 시간)

- 읽기에는 딱 한 단계만 필요하기 때문에 O(1)이라고 표현
- 입력 데이터 크기에 상관없이 언제나 일정한 시간이 걸림
- 즉, n의 값이 커져도 실행시간은 변하지 않음

```javascript
function addUpTo(n) {
  return (n * (n + 1)) / 2;
}
```

### O(n)

Linear Time(선형 시간)

- 문제를 해결하기 위한 단계의 수와 n이 1:1 관계를 가짐

```javascript
function addUpTo(n) {
  let total = 0;
  for (let i = 1; i <= n; i++) {
    total += i;
  }
  return total;
}
```

### O(n^2)

Quadratic Time(이차 시간)

- 데이터가 n만큼 반복하는데, 그 안에서 n만큼 또 반복할 때의 표기 방법(중첩 루프)

```javascript
function printAllPairs(n) {
    for (var i =0; i<n; i++>) {
        for (var j = 0; j<n; j++) {
            console.log(i,j)
        }
    }
}
```

## 공간 복잡도

- 알고리즘이 실행될 때 사용되는 메모리의 양
- 입력이 커질수록 알고리즘이 얼마나 많은 공간을 차지하게 되는가

### O(1)

- 변수가 total과 i 두개 밖에 없기 때문에 O(1)

```javascript
function sum(arr) {
  let total = 0;
  for (let i = 0; i < arr.length; i++) {
    total += arr[i];
  }
  return total;
}
```

### O(n)

- 입력된 배열의 길이와 새로운 배열에 저장되는 아이템의 길이가 비례

```javascript
function double(arr) {
  let newArr = [];
  for (let i = 0; i < arr.length; i++) {
    newArr.push(2 * arr[i]);
  }
  return newArr;
}
```

## 빅오 표현식 단순화하기

O(2n) => O(n) </br>
O(500) => 0(1) </br>
O(13n^2) => O(n^2) </br>
O(n + 10) => O(n) </br>
O(1000n + 50) => O(n) </br>
O(n^2 + 5n + 8) => O(n^2)

## 로가리즘

O(lonN)을 이해하려면 먼저 로가리즘을 이해해야 한다. 로가리즘은 지수와 역의 관계이다.

<img src="images/로가리즘.png">

## 객체의 빅오

삽입 : O(1) </br>
제거 : O(1) </br>
검색 : O(N) </br>
접근 : O(1) </br>

## 배열의 빅오

삽입 : 어느 위치에 삽입하느냐에 따라 다름! </br>
제거 : 어느 위치에서 제거하느냐에 따라 다름! </br>
검색 : O(N) </br>
접근 : O(1) </br>

### 배열 매서드의 빅오

push - O(1) </br>
pop - O(1) </br>
shift - O(N) </br>
unshift - O(N) </br>
concat - O(N) </br>
slice - O(N) </br>
splice - O(N) </br>
sort - O(N \* log N) </br>
forEach/map/filter/reduce/etc. - O(N)</br>
