<br>

# 📌  문제 해결 패턴

<br>

## 🔎  빈도수 세기 패턴

<br>

👉  두 배열이 주어졌을 때, 한 배열은 다른 배열의 2배수를 가지고 있는지 판별해야 하는 문제가 주어졌다.

이를 해결하기 위해서 무의식적으로 이중 for문을 사용할 것이다.

<br>

```tsx
function same(arr1, arr2) {
  if (arr1.length !== arr2.length) {
    return false;
  }

  for (let i = 0; i < arr1.lengtgh; i++) {
    // arr2가 현재 arr[i]에 해당하는 2배수를 가지고 있는지 확인 후 그 인덱스를 반환
    let correctIndex = arr2.indexOf(arr[i] * 2);

    // 만약 없다면 판별 종료
    if (correctIndex === -1) {
      return false;
    }

    // 현재 요소는 있기 때문에 지워준다.
    arr2.splice(correctIndex, 1);
  }

  // 여기까지 도달했다는 것은 모든 체크가 끝났다는 것이기 때문에 판별 종료
  return true;
}
```

<br>

하지만 위의 코드는 시간 복잡도가 O(n^2)이다. 이를 코테에서 이용하면 광탈이다…

자 그럼 어떻게 해결할 수 있을까??

 <br>

```tsx
function same(arr1, arr2) {
  if (arr1.length !== arr2.length) {
    return false;
  }

  const obj = {};

  for (let num of arr1) {
    obj[num] = (obj[num] || 0) + 1;
  }

  for (let num of arr2) {
    if (obj[num / 2]) {
      obj[num / 2] -= 1;
    } else {
      return false;
    }
  }

  return true;
}
```

<br>

위처럼 객체를 이용하여 key와 빈도수를 체크하면 O(n)으로 시간복잡도를 줄일 수 있다.

<br>

## 🔎  슬라이딩 윈도우

<br>

👉  한 개의 배열 arr와 양의 정수 n이 주어진다. 이때 arr의 요소 중 n개의 최대합을 구해야 한다면 어떻게 해결해야 할까??

<br>

```tsx
function maxSubarraySum(arr, n) {
  if (arr.length < n) return null;

  let maxSum = 0;
  let tempSum = 0;

  for (let i = 0; i < n; i++) {
    maxSum += arr[i];
  }
  tempSum = maxSum;
  for (let i = n; i < arr.length; i++) {
    tempSum = tempSum - arr[i - n] + arr[i];
    maxSum = Math.max(maxSum, tempSum);
  }
  return maxSum;
}
```

<br>

이중 for문을 통해 모든 구간들을 살펴보는게 아니라, 첫 구간에서 앞의 한 개와 뒤의 한 개를 교체해가면서 값을 구하는 방식이 슬라이딩 윈도우이다.

<br>
