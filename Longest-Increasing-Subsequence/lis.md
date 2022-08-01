# Longest Increasing Subsequence (LIS)

## 최장 증가 수열 (최장 증가 부분 수열)

어떠한 수열의 부분 수열 중에서 **오름차순으로 증가하는 가장 긴 부분수열**을 말한다.

<br/>

주어진 수열에서 LIS의 길이(length)를 구하는 여러 방법이 있다.

- Recursive Approach (Brute Force)
- Dynamic Programming Approach
- Greedy Approach
- Greedy + Binary Search

이 중에서 Recursive Approach와 Dynamic Programming Approach 에 대해 살펴보자.

<br/>

### 1. Recursive Approach (Brute Force)

- 완전 탐색 알고리즘
- 완전 탐색을 하기 때문에 절대로 틀릴 수 없는 알고리즘
- 그렇기 때문에 속도가 느림
- Brute Force Algorithm 에는 대표적으로 DFS, BFS 가 있다.
- Time Complextity `O(2^N)`

```js
// naive solution: O(2^N)
// 배열의 각 요소에 대해서 선택, 무시의 2가지 선택이 가능
const LIS = function (arr) {
	// 현재 검토할 차례인 배열의 '인덱스'와
	// 이전에 선택된 요소의 '값'을 인자로 전달한다.
	const pickOrNot = (idx, before) => {
		// base case
		// 가장 짧은 LIS의 길이는 1이다. 모든 요소는 그 자체로 길이 1인 부분 서열이다.
		if (idx === arr.length) return 1;

		// recursive case
		// (초기값인 Number.MAX_SAFE_INTEGER를 포함해) 이전에 선택된 요소와 비교를 한다.
		const adder = arr[idx] > before ? 1 : 0;
		return Math.max(
			// 1) 현재 요소를 선택한다.
			//  1-1) adder === 1: 현재 요소를 이전에 선택된 요소 뒤에 이어지는 요소로 생각해 LIS의 길이에 1을 더한다.
			//  1-2) adder === 0: 현재 요소를 이어지는 요소로 생각할 수 없는 경우. 이전 요소를 건너뛰고 LIS의 처음 또는 중간 요소로 현재 요소를 선택한다.
			adder + pickOrNot(idx + 1, arr[idx]), // concat or restart
			// 2) 현재 요소를 무시한다.
			pickOrNot(idx + 1, before) // ignore
		);
	};
	// 첫 번째 요소의 이전 요소는 없기 때문에 매우 큰 값을 이전 값으로 설정한다.
	// 첫 번째 요소부터 시작하는 LIS를 검사하는 효과를 갖는다.
	return pickOrNot(0, Number.MAX_SAFE_INTEGER);
};
```

### 2. Dynamic Programming Approach

- Time Complexity `O(N^2)`
- strictly increasing subsequence 를 요구하기 때문에, 중복되는 요소는 없다.
- 요소의 길이를 리턴하는 것이기 때문에 subsequence를 저장하지는 않는다.

<br/>

실행 순서:

1. 먼저 길이가 `N`인 1차원 배열 `lis[]` 를 만든다.
2. index 1 부터 index N-1 까지 모든 요소를 순회한다.
3. 중첩된 반복문으로 현재 element의 index 보다 작은 index를 가진 요소들을 순회한다.
4. `arr[i] > arr[j]` && `lis[i] < lis[j] + 1` 라면 `lis[i] = lis[j] + 1` 이다.
5. `lis[]` 중 가장 큰 요소를 반환한다.

이것을 코드로 작성하면 아래와 같다.

```js
// dynamic programming with tabulation: O(N^2)
const LIS = function (arr) {
	const N = arr.length;
	// lis[i]는 i에서 끝나는 LIS의 길이를 저장
	// 최소한 각 요소 하나로 LIS를 만들 수 있으므로 1로 초기화한다.
	const lis = Array(N).fill(1);
	for (let i = 1; i < N; i++) {
		// i에서 끝나는 LIS의 길이
		for (let j = 0; j < i; j++) {
			// i 이전의 인덱스만 검사하면 된다.
			// i는 1부터 시작하므로, 짧은 길이부터 검사한다. (bottom-up 방식)
			if (arr[i] > arr[j] && lis[i] < lis[j] + 1) {
				lis[i] = lis[j] + 1;
			}
		}
	}
	return Math.max(...lis);
};
```

```js
// dynamic programming with memoization: O(N^2)
const LIS = function (arr) {
	// memo[i]는 i부터 시작하는 LIS의 길이를 저장
	const memo = Array(arr.length).fill(-1);
	// 마지막 요소부터 시작하는 LIS는 1이 유일하다.
	memo[memo.length - 1] = 1;
	const calculateLIS = (idx) => {
		if (memo[idx] !== -1) return memo[idx];

		let max = 1;
		for (let i = idx + 1; i < arr.length; i++) {
			const len = calculateLIS(i);
			// idx와 i가 연결되지 않을 수도 있다.
			if (arr[idx] < arr[i]) {
				// i부터 시작하는 LIS를 연결할 수 있는 경우
				max = Math.max(max, len + 1);
			}
			// i부터 시작하는 LIS가 더 길 수도 있다.
			// idx부터 시작하는 LIS를 구해야 하므로, 무시한다.
		}
		memo[idx] = max;
		return memo[idx];
	};
	calculateLIS(0);
	// 가장 긴 길이를 구한다.
	return Math.max(...memo);
};
```

<br/>

### References

- https://afteracademy.com/blog/longest-increasing-subsequence
- https://www.youtube.com/watch?v=cjWnW0hdF1Y&ab_channel=NeetCode
- https://4legs-study.tistory.com/106
