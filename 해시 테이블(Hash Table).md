# 📚 해시 테이블(Hash Tables)

![](https://minho-jang.github.io/assets/images/dev/hash-3.png)

- 키-값 쌍을 저장하는데 사용된다.
- 배열과 비슷하지만 키(key)는 순서대로 있지 않다.
- 값을 찾거나, 추가하거나, 제거하는데 있어서 매우 빠르다.
- 속도가 아주 빠르게 때문에 거의 모든 프로그래밍 언어가 해시 테이블 구조를 가지고 있다.

# 📚 해시 함수(Hash function)

- 임의의 크기를 가진 데이터를 입력하면 정해진 크기 안에서의 데이터를 출력한다. 입력값과 관계없이 데이터는 비슷한 크기를 가진다.
- 문자열로 된 키를 인덱스(작은 숫자로 바꿔주는데 사용)바꿔준다.
- 정보 보호, 저장, 사이트 로그인 인증, 암호화폐 등에 사용된다.
- 결과값만 출력할 수 있으며 그 반대의 작업은 할 수 없다. (단방향 함수)

## 💡 좋은 해시 함수를 결정하는 기준

1. 빨라야 한다. (ex. 상수 시간)
2. 일관된 방식으로 값을 고르게 분배해서 보관 장소를 겹치지 않게 해야 한다.
3. 좋은 해시 함수는 결정론적이다.
   - 특정 입력값을 입력할 때마다 같은 출력값이 나와야한다. 즉 출력값은 이미 결정되어 있어야 한다.

## 📌 해시 함수 작성

```javascript
// 문자열에서만 작동하는 해시 함수
function hash(key, arrayLen) {
  // 해시 값 저장할 변수 초기화
  let total = 0;
  // 키의 각 문자에 대해 반복
  for (let char of key) {
    // 각 문자의 값은 현재 문자의 ASCII 값에서 ASCII 값 (97)을 빼서 계산된다.
    // "a"를 1로, "b"를 2로, "c"를 3으로 매핑한다.
    let value = char.charCodeAt(0) - 96;
    // value를 total에 더하고, 그 후 모듈로 연산(% arrayLen)을 수행하여 해시 값을 지정된 배열 길이 범위 내에서 출력한다.
    total = (total + value) % arrayLen;
  }
  return total;
}
```

=> 해당 함수의 문제점

- 오직 문자열에서만 해시할 수 있다.
- 함수 실행의 시간이 O(1)이 아니라 O(N)이다.
- 무작위성이 떨어지고, 해시된 인덱스가 충돌할 수 있다.

## 📌 해시 함수 성능 향상 시키기

```javascript
function hash(key, arrayLen) {
  let total = 0;
  // 충돌 횟수를 줄이고 데이터를 더 퍼뜨리기 위해 소수를 추가한다.
  let WEIRD_PRIME = 31;
  // 문자열이 수백만 글자라고 했을 때
  // 속도를 향상 시키기 위해 수백만 번의 루프 대신 앞글자 100개만 반복하도록 지정한다.
  for (let i = 0; i < Math.min(key.length, 100); i++) {
    let char = key[i];
    let value = char.charCodeAt(0) - 96;
    total = (total * WEIRD_PRIME + value) % arrayLen;
  }
  return total;
}
```

## 📌 충돌 해결하기

### ✅ Separate Chaining(개별 체이닝)

![](<https://velog.velcdn.com/images%2Fjangws%2Fpost%2F1a58d7fd-9901-4c97-96c1-d44ff5ba3bcd%2FUntitled%20(20).png>)

- 같은 테이블에 데이터를 여러개 저장할 때 배열이나 연결 리스트 등과 같은 것을 활용해서 이중 데이터 구조를 쓰는 것이다. 즉, 여러 개의 키-값 쌍을 같은 자리에 저장할 수 있다.(중첩 저장)
- 테이블의 길이보다 더 많은 데이터를 저장할 수 있다.

### ✅ Linear Probing(선형 탐색법)

![](<https://velog.velcdn.com/images%2Fjangws%2Fpost%2Ffa864e48-8496-4f45-a8b1-c9aaa1245909%2FUntitled%20(21).png>)

- 각 위치에 하나의 데이터만 저장한다는 규칙을 이용한다.
- 충돌이 발생하면(테이블에 데이터가 겹치면) 다음 빈 테이블을 찾아 데이터를 저장한다.
- 하나의 테이블에 하나의 데이터만 저장되므로 공간이 부족하다.

## 📌 해시 테이블 - 개별 체이닝을 이용한 Set, Get 메서드

```javascript
class HashTable {
  // size는 해시 테이블의 길이를 결정
  // 소숫값으로 53설정
  constructor(size = 53) {
    this.keyMap = new Array(size);
  }

  _hash(key) {
    let total = 0;
    let WEIRD_PRIME = 31;
    for (let i = 0; i < Math.min(key.length, 100); i++) {
      let char = key[i];
      let value = char.charCodeAt(0) - 96;
      total = (total * WEIRD_PRIME + value) % this.keyMap.length;
    }
    return total;
  }
  // set 메서드
  set(key, value) {
    // 키의 해시 값 구하기
    let index = this._hash(key);
    // 인덱스에 값이 없다면, 빈 배열 만들기
    // 이 배열은 인덱스의 키-값을 저장하는데 사용
    if (!this.keyMap[index]) {
      this.keyMap[index] = [];
    }
    // 인덱스에 값이 있다면
    // 배열에 키-값 쌍을 추가
    this.keyMap[index].push([key, value]);
  }
  // get 메서드
  // 키에 해당하는 값 찾기
  get(key) {
    // 키의 해시 값 구하기
    let index = this._hash(key);
    // 인덱스에 값이 있다면(중첩)
    if (this.keyMap[index]) {
      // 해당 인덱스의 배열을 반복하면서 키 찾기
      for (let i = 0; i < this.keyMap[index].length; i++) {
        // 현재 요소의 0번째 값(키)이 주어진 키와 같다면
        if (this.keyMap[index][i][0] === key) {
          // 1번째 값(값) 반환
          return this.keyMap[index][i][1];
        }
      }
    }
    // 주어진 키에 해당하는 값이 없으면 undefined 반환
    return undefined;
  }
}
```

## 📌 해시 테이블 - Key, Value 메서드

```javascript
// 테이블에 있는 모든 key들의 목록에서 중복을 제거하고 한개만 반환
 keys(){
    let keysArr = [];
    // 테이블 순회
    for(let i = 0; i < this.keyMap.length; i++){
        // 현재 인덱스에 값이 있다면 하위 배열을 순회하여 키값 확인
      if(this.keyMap[i]){
        for(let j = 0; j < this.keyMap[i].length; j++){
            // 동일한 키가 배열에 포함되지 않을경우(중복x) 추가
          if(!keysArr.includes(this.keyMap[i][j][0])){
            keysArr.push(this.keyMap[i][j][0])
          }
        }
      }
    }
    return keysArr;
  }
  // keys와 동일
  values(){
    let valuesArr = [];
    for(let i = 0; i < this.keyMap.length; i++){
      if(this.keyMap[i]){
        for(let j = 0; j < this.keyMap[i].length; j++){
          if(!valuesArr.includes(this.keyMap[i][j][1])){
            valuesArr.push(this.keyMap[i][j][1])
          }
        }
      }
    }
    return valuesArr;
  }
```

## 📌 해시 테이블의 Big O

- 삽입 : O(1)
- 제거 : O(1)
- 접근 : O(1)
