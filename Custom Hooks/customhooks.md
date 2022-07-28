# React Custom Hooks

⭐️ 공식 문서 참고: https://ko.reactjs.org/docs/hooks-custom.html

React에서 custom hook 이란 **개발자가 직접 커스텀한 훅**을 말한다.

<br/>

## Custom Hooks을 사용하면 좋은 점

- 상태 관리 로직의 재활용이 가능하다
- class component 보다 간결하게 같은 로직을 구현할 수 있다
- functional component로 작성하기 때문에 명료하고 이해가 쉽다

<br/>

## Custom Hook의 규칙

- 앞에 `use` 를 붙이는 것이 관습이다. (e.g. `useSomething`)
- 대부분의 경우에는 custom hook들을 프로젝트 내의 hooks 디렉토리를 만들어 이곳에 위치시킨다.
- custom hook을 만들 때 함수는 return 값이 조건부여서는 안된다. (custom hook은 조건부 함수이면 안된다.)

<br/>

## Custom Hook을 사용하면 좋은 경우 예시

아래의 코드는 React 공식문서에서 발췌했다.

```js
//FriendStatus : 친구가 online인지 offline인지 return하는 컴포넌트
function FriendStatus(props) {
	const [isOnline, setIsOnline] = useState(null);
	useEffect(() => {
		function handleStatusChange(status) {
			setIsOnline(status.isOnline);
		}
		ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
		return () => {
			ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
		};
	});

	if (isOnline === null) {
		return 'Loading...';
	}
	return isOnline ? 'Online' : 'Offline';
}

//FriendListItem : 친구가 online일 때 초록색으로 표시하는 컴포넌트
function FriendListItem(props) {
	const [isOnline, setIsOnline] = useState(null);
	useEffect(() => {
		function handleStatusChange(status) {
			setIsOnline(status.isOnline);
		}
		ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
		return () => {
			ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
		};
	});

	return (
		<li style={{ color: isOnline ? 'green' : 'black' }}>{props.friend.name}</li>
	);
}
```

<br/>

위의 `FriendStatus` 컴포넌트와 `FriendListItem` 컴포넌트는 동일한 로직을 가지고 있다. 이런 경우 아래와 같이 custom hook을 사용해서 이 로직을 함수로 뽑아내서 재사용할 수 있다.

`useFriendStatus` 라는 custom hook을 만들어서 위의 중복된 로직을 담는다.

```js
function useFriendStatus(friendID) {
	const [isOnline, setIsOnline] = useState(null);

	useEffect(() => {
		function handleStatusChange(status) {
			setIsOnline(status.isOnline);
		}

		ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
		return () => {
			ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
		};
	});

	return isOnline;
}
```

<br/>

이제 이 `useFriendStatus` hook을 `FriendStatus` 컴포넌트와 `FriendListItem` 컴포넌트에 적용하면 아래와 같다.
custom hook을 사용하지 않았을 때와 비교하면 코드가 훨씬 간결하고 직관적이다.

주의할 것은 이 경우 같은 custom hook을 사용했지만, 그렇다고 해서 `FriendStatus` 컴포넌트와 `FriendListItem` 컴포넌트가 같은 state 를 공유하는 것은 아니다.

**각자의 컴포넌트 내에서 state 는 독립적으로 정의되어 있다.**

```js
function FriendStatus(props) {
	const isOnline = useFriendStatus(props.friend.id);

	if (isOnline === null) {
		return 'Loading...';
	}
	return isOnline ? 'Online' : 'Offline';
}

function FriendListItem(props) {
	const isOnline = useFriendStatus(props.friend.id);

	return (
		<li style={{ color: isOnline ? 'green' : 'black' }}>{props.friend.name}</li>
	);
}
```

<br/>

## Custom Hook 의 예시

`useFetch`: 여러 url을 fetch할 때 사용

```js
const useFetch = (initialUrl: string) => {
	const [url, setUrl] = useState(initialUrl);
	const [value, setValue] = useState('');

	const fetchData = () => axios.get(url).then(({ data }) => setValue(data));

	useEffect(() => {
		fetchData();
	}, [url]);

	return [value];
};

export default useFetch;
```

`useInputs` : 여러 input에 의한 상태 변경을 할 때 사용

```js
import { useState, useCallback } from 'react';

function useInputs(initialForm) {
	const [form, setForm] = useState(initialForm);
	// change
	const onChange = useCallback((e) => {
		const { name, value } = e.target;
		setForm((form) => ({ ...form, [name]: value }));
	}, []);
	const reset = useCallback(() => setForm(initialForm), [initialForm]);
	return [form, onChange, reset];
}

export default useInputs;
```
