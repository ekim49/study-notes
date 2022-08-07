# Insertion Sort

삽입 정렬은 가장 첫 번째 element는 정렬이 되어있다고 가정을 하고 두 번째 element 부터 주어진 배열의 끝까지 순회한다.

즉, 기준이 되는 element의 이전에 있는 element들이 정렬이 되어있다고 **가정**을 하고, 적절한 위치에 삽입하는 알고리즘이다.

가정을 하기 때문에 버블, 선택 정렬에 비해서 효율적이라고 말할 수 있다.

버블 정렬과 선택 정렬과는 다르게 삽입 정렬은 필요할 때만 위치를 바꾸기 때문에 정렬을 수행하는 것이 더 빠르다.

<br/>

### Time Complexity

|  Best  | Average  |  Worst   | Space Complexity |
| :----: | :------: | :------: | :--------------: |
| `O(n)` | `O(n^2)` | `O(n^2)` |      `O(1)`      |

<br/>

### Insertion Sort 작성 예시

이중 반복문으로 삽입 정렬을 구현할 수 있는데, 주의할 점은 첫 번째 반복문(outer loop)의 진행방향과, 두 번째 반복문(inner loop)의 진행방향은 반대이다.

```js
function insertionSort(array) {
	for (let i = 1; i < array.length; i++) {
		for (let j = i; j > 0; j--) {
			// j를 인덱스로 사용하기 위해 j = i 를 반복문의 초기값으로 설정한다.
			// 각 element를 이미 정렬된 element들과 비교해서 어디에 삽입할지 찾기 위해 역방향으로 순회한다.
			if (array[j] < array[j - 1]) {
				// 현재 element가 직전 element 보다 값이 작다면,
				const temp = array[j]; // 둘의 위치를 바꾼다. swap
				array[j] = array[j - 1];
				array[j - 1] = temp;
			} else {
				break;
			}
		}
	}
	return array;
}
```

<br/>

위의 코드를 리팩토링 하면 아래와 같다. (ES6 구조 분해 할당 사용)

```js
function insertionSort(array) {
	for (let i = 1; i < array.length; i++) {
		for (let j = i; j > 0; j--) {
			if (array[j] < array[j - 1]) {
				[array[j], array[j - 1]] = [array[j - 1], array[j]];
			} else {
				break;
			}
		}
	}
	return array;
}
```

<br/>

위 함수는 원본 배열을 mutate 하므로 순수함수가 아니다.

```js
const arr = [1, 4, 2, 7, 3, 89, 12, 23, 32, 19, 6, 41, 17, 45];

console.log(insertionSort(arr));
console.log(arr); // 원본 배열이 mutate 되었음을 알 수 있다.
```

insertion sort를 순수함수로 구현하면 아래와 같다.

<br/>
<br/>

## ✅ Insertion Sort를 순수함수로 구현

위의 insertionSort 함수를 순수함수로 만들기 위해서는 아래 처럼 원본 배열을 **shallow copy** 해서 복사본을 만들어 함수를 작성할 수 있다.

```js
function insertionSort(array) {
	const arr = array.slice(); // shallow copy

	for (let i = 1; i < arr.length; i++) {
		for (let j = i; j > 0; j--) {
			if (arr[j] < arr[j - 1]) {
				[arr[j], arr[j - 1]] = [arr[j - 1], arr[j]];
			} else {
				break;
			}
		}
	}
	return arr;
}
```

이렇게 순수함수로 작성하면 원본 배열은 mutate 되지 않는다.
즉, parameter 로 전달 받은 array 는 변경되지 않는다.

```js
const arr = [1, 4, 2, 7, 3, 89, 12, 23, 32, 19, 6, 41, 17, 45];

console.log(insertionSort(arr));
console.log(arr); // 원본 배열이 mutate 되지 않았음을 알 수 있다.
```
