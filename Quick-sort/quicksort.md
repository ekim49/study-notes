# Quick Sort

- Merge Sort와 함께 Quick Sort는 **Divide and Conquer 알고리즘**이다.

* in place 방법 → 많이 쓰이는 방법, 메모리 사용량이 적다.
* not in place 방법 → 별도의 메모리 공간이 필요, 데이터의 야이 많을수록 메모리 소모가 증가하여 비효율적이므로 잘 쓰이지 않는 방법이다.

<br/>

### Quick Sort 실행 순서

1. pivot element 하나를 정한다.
2. 주어진 배열에서 pivot을 제외한 모든 요소들을 탐색해서, pivot 보다 작으면 left, 크면 right에 둔다.
3. 주어진 배열의 모든 요소를 탐색해서 left, right으로 분류 완료하면, 각각의 left, right 을 재귀적으로 퀵 정렬을 한다.
4. left, pivot, right 을 합친다. (spread operator 사용)

```js
function quickSort(array) {
	// base case
	if (array.length === 1) {
		return array;
	}

	// recursive case
	const pivot = array[array.length - 1]; // 가장 마지막 원소를 pivot으로 정한다.
	const leftArr = []; // pivot 기준 왼쪽에 놓일 element를 담을 배열을 만든다.
	const rightArr = []; // pivot 기준 오른쪽에 놓일 element를 담을 배열을 만든다.

	for (let i = 0; i < array.length - 1; i++) {
		// pivot은 반복문에서 포함할 필요가 없다
		if (array[i] < pivot) {
			leftArr.push(array[i]);
		} else {
			rightArr.push(array[i]);
		}
	}

	// 만약 가장 값이 큰 element를 pivot으로 정했을 경우, rightArr 의 배열은 비어있게 되므로 이렇게 처리한다.
	if (leftArr.length > 0 && rightArr.length > 0) {
		return [...quickSort(leftArr), pivot, ...quickSort(rightArr)];
	} else if (leftArr.length > 0) {
		// rightArr 가 없는 경우
		return [...quickSort(leftArr), pivot];
	} else {
		// leftArr 가 없는 경우
		return [pivot, ...quickSort(rightArr)];
	}
}
```

```js
const arr = [1, 10, 5, 8, 7, 6, 4, 3, 2, 9];

console.log(quickSort(arr)); // quickSort는 원본 배열을 mutate 않았으므로, 순수함수 이다.
console.log(arr);
```

<br/>
위의 quickSort 코드를 리팩토링한다면 아래와 같다.
- for ... of 사용
- ternary operator 사용

```js
function quickSort(array) {
	if (array.length <= 1) {
		return array;
	}

	const pivot = array[array.length - 1];
	const leftArr = [];
	const rightArr = [];

	// 가장 마지막 element가 pivot이므로, 포함시키지 않게 하기 위해 아래와 같이 for...of 반복문을 쓸 수 있다.
	for (const el of array.slice(0, array.length - 1)) {
		// ternary operator 사용
		el < pivot ? leftArr.push(el) : rightArr.push(el);
	}

	return [...quickSort(leftArr), pivot, ...quickSort(rightArr)];
}
```
