# Hash Table

해시 : 단방향 암호화 기법.
임의의 길이의 데이터를 고정된 길이의 데이터(암호화된 문자열)로 매핑하고 내부적으로 배열(버킷)을 사용하여 데이터(value)를 저장한다.

- 해시함수를 사용하여 특정 값을 신속하게 찾는 자료구조. O(1)
- 다양한 가변 길이의 입력에 대해 고정된 크기의 결과값을 반환.
- 결과값을 바탕으로 입력 값을 찾는 것이 불가능.(역추적이 어렵다)

## 해시 함수

### loselose

특정 문자열이 들어왔을 때 문자열을 하나씩 아스키코드로 변환하여 해시 값으로 변환하고 그 값을 인덱스로써 사용한다. (Division Method)

```js
const HASH_SIZE = 1013; // 범위지정

class Element {
  constructor(key, value) {
    this.key = key;
    this.value = value;
  }
}

class HashTable {
  constructor() {
    this.table = new Array(HASH_SIZE);
    this.length = 0;
  }

  // 입력된 Key값을 바탕으로 해쉬코드 생성
  hashCode(key) {
    let hash = 0;
    for (let i = 0; i < key.length; i++) {
      hash += key.charCodeAt(i); // 유니코드, 아스키코드와 같이 문자열이 가지고 있는 수의 값을 누적해서 저장한다. (Digit Folding)
    }
  }
}
```

### djb2

```js
/* djb2 hash(위쪽 HASH_SIZE까지 확인) */
hashCode(key) {
  let hash = 5381; //seed(소수)
  for (let i = 0; i < key.length; i++) {
    hash += hash * 33 + key.charCodeAt(i); // (seed(소수) * 33)까지 더해서 충돌이 나지 않도록.
  }
  return hash % HASH_SIZE; // 37이내의 수로 변환
}
```

---

## 충돌 방지

해시 함수는 입력값의 길이가 어떻든 고정된 길이의 값을 출력하기 때문에 입력값이 다르더라도 같은 결과값이 나오는 경우가 있다.(생성된 해시코드의 인덱스에 이미 값이 저장되어 있는 경우)

### 체이닝

1. 개방 주소법
   비어있는 해시 테이블의 공간을 활용하는 방법.

- 선형 탐사 : 현재의 버킷 index로부터 고정폭 만큼씩 이동하여 차례대로 검색해 비어 있는 버킷에 데이터를 저장한다. 인덱스를 조정하여 남은 인덱스에 추가될 수 있도록하고 남은 인덱스가 존재하지 않는 경우에 false를 반환한다.

2. 분리 연결법
   동일한 버킷의 데이터에 대해 자료구조를 활용해 추가 메모리를 사용, 다음 데이터의 주소를 저장한다. 만약 동일한 버킷으로 접근시 데이터들을 연결해서 관리한다.
