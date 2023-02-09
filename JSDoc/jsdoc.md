# JSDoc

> JavaScript의 API를 문서화하는 툴이다. JSDoc에서 제공하는 **태그**를 이용해서 클래스, 함수, 모듈, 메서드, 파라미터 등을 문서화한다. 여러 종류의 태그들이 있어서 잘 사용하면 구체적인 정보가 담긴 문서를 만들 수 있다.

## JSDoc의 태그 종류

[jsdoc.app](https://jsdoc.app/)

[JSDoc 3.6 한국어](https://runebook.dev/ko/docs/jsdoc/-index-)

<br/>
<br/>

# 📌 JSDoc 사용 방법

주석을 코드와 함께 소스코드에 작성하면, JSDoc이 소스코드를 기반으로 HTML 문서를 생성한다.

주석을 작성할 때 `/** 주석 */` 와 같이 입력하면 JSDoc이 인식할 수 있는 주석이 된다.

작성하는 주석의 예시는 아래와 같다.

```tsx
/**
 * 두 개의 숫자를 받아서 합한 값을 리턴하는 add 함수
 * @param {number} number1 - 첫 번째 숫자
 * @param {number} number2 - 두 번째 숫자
 * @returns {number} number1 + number2
 */
function add(number1, number2) {
	return number1 + number2;
}
```

## 초기 환경 세팅 / 설치

원하는 디렉토리에 JSDoc을 만들 새 폴더 생성

```bash
# jsdoc-practice 폴더 생성, 진입
mkdir jsdoc-practice
cd jsdoc-practice

# package.json 생성
npm init -y

# src 폴더 생성, 진입
mkdir src
cd src

# index.js 파일 생성
touch index.js

# jsdoc 설치 (전역으로 설치하면 명령어가 간단하다)
npm install -d jsdoc
npm install -g jsdoc
```

<br/>

## JSDoc 주석 작성

생성한 index.js에 위의 예시를 옮겨 적어보자.

<img src='https://velog.velcdn.com/images/hailieejkim/post/b50ec9e9-1a12-4c83-a6b6-27b1e86598f0/image.png' width=500px>

JSDoc을 사용하면 `타입 체킹` 도 가능하다.

<img src='https://velog.velcdn.com/images/hailieejkim/post/92fff798-f738-4e84-9ea6-47b73c2e0a7f/image.png' width=500px>

<br/>

터미널에 아래와 같이 jsdoc으로 변환해주는 명령어를 입력하면 디렉토리에 `out` 폴더가 생성된다.

```bash
jsdoc ./src/index.js
```

<img src='https://velog.velcdn.com/images/hailieejkim/post/5278272a-d25f-49f9-8cf6-dff6e6499d21/image.png' width=300px>

`out/index.html` 을 오른쪽 클릭해서 브라우저에서 열어보면 문서화된 JSDoc을 볼 수 있다.

<img src='https://velog.velcdn.com/images/hailieejkim/post/9bab1a9f-346e-4f2e-af11-0c8ad25c2a6a/image.png' width=500px>

중간 쯤에 있는 파란색 index.js 링크를 클릭하면 작성한 소스코드도 확인할 수 있다.

<img src='https://velog.velcdn.com/images/hailieejkim/post/8b2f8c13-3672-4fe6-bc8b-fdc1ff8a94db/image.png' width=500px>

<br/>
<br/>

# 📌 여러개의 .js 파일을 합쳐서 하나의 JSDoc 문서 만들기

보통 `src` 폴더 내에 여러개의 `.js` 파일들이 있다.

이 모든 파일들이 합쳐진 JSDoc을 만들기 위해서는 아래와 같이 `jsdoc.config.json` 파일을 만든다.

## jsdoc.config.json

root 디렉토리에 `jsdoc.config.json` 파일을 만들고, 아래와 같이 기본 설정을 한다.

```json
{
	"plugins": ["plugins/markdown"],
	"recurseDepth": 10,
	"source": {
		"include": [".", "src"],
		"includePattern": ".+\\.js(doc|x)?$",
		"excludePattern": "(^|\\/|\\\\)_"
	},
	"sourceType": "module",
	"tags": {
		"allowUnknownTags": true,
		"dictionaries": ["jsdoc", "closure"]
	},
	"templates": {
		"cleverLinks": false,
		"monospaceLinks": false
	},
	"opts": {
		"encoding": "utf8",
		"readme": "README.md"
	}
}
```

<br/>

아래 명령어를 입력하면 하나로 합쳐진 JSDoc 문서를 볼 수 있다. (JSDoc 브라우저를 다시 열거나 새로고침)

```bash
./node_modules/.bin/jsdoc -c jsdoc.config.json .
```

<img src='https://velog.velcdn.com/images/hailieejkim/post/a8bb41d2-4ead-4476-8432-638ceb4577d4/image.png' width=500px>

<br/>
<br/>

# JSDoc에 README.md 추가하기

<img src='https://velog.velcdn.com/images/hailieejkim/post/47ce741f-1d48-4685-8cdc-a15c7de82a46/image.png' width=500px>

JSDoc의 Home이 비어있다.

여기에 `README.md` 파일을 넣을 수 있다.

이 문서를 소개하는 내용을 담아 `README.md` 파일을 작성해서 Home에서 보여주면 좋지 않을까?

<br/>

## README.md

`root` 디렉토리에 `README.md` 파일을 생성하고 내용을 입력한다.

```md
# JSDoc 문서 제목

## 💁🏻‍♀️ 문서 소개

JSDoc 작성하는 방법을 연습해보는 문서입니다.
잘 작성해 봅시다! ✨
```

<br/>

저장 후 다시 아래 명령어를 입력해서 문서를 만들면 `README.md` 가 반영된 JSDoc 을 확인할 수 있다.

```bash
./node_modules/.bin/jsdoc -c jsdoc.config.json .
```

<img src='https://velog.velcdn.com/images/hailieejkim/post/f1cd7938-55c2-46af-9419-26ac90209443/image.png' width=700px>

<br/>
<br/>

# 📌 JSDoc을 Github Pages 로 배포하기

너무나도 당연한 이야기지만 API 문서는 **공유**가 필요한 문서다.

쉽게 공유하려면 JSDoc을 배포하면 된다.

만든 JSDoc 정적 페이지를 Github Pages로 배포해보자.

_(역시 당연한 이야기지만, Github Pages 로 배포하기 위해서는 위에서 만든 jsdoc-practice 파일이 Github 레포지토리에 최종 push 되어있어야 한다.)_

<br/>

## gh-pages 설치

먼저 gh-pages 패키지를 설치한다.

```json
npm install --save gh-pages
```

<br/>

## package.json 수정

package.json 파일을 내부에 homepage를 아래와 같이 추가해서 수정한다.

```json
// "homepage": "https://깃헙아이디.github.io/레포지토리이름"
"homepage": "https://ekim49.github.io/wanted-pre-onboarding-challenge-fe-ts"
```

package.json 파일 내부의 scripts 를 수정한다.

```json
"scripts": {
	"docs": "jsdoc -c jsdoc.json",
	"predeploy": "npm run docs",
	"deploy": "gh-pages -d docs"
}
```

<br/>

## 배포하기

script 수정 후 터미널에 아래 명령어를 입력해서 배포한다.

```bash
npm run deploy
```

그 다음 Github의 해당 레포지토리의 `Settings → Pages` 탭으로 이동한다.

<img src='https://velog.velcdn.com/images/hailieejkim/post/e8e5918b-53d5-4c09-b9f9-9affe955771d/image.png' width=700px>

Source 를 `Deploy from a branch` 로 선택하고,

Branch 는 `gh-pages` 로 설정한다.

그리고 위의 visit site 버튼을 클릭하거나, 링크를 클릭하면 배포된 JSDoc 을 확인할 수 있다. 👏🏼👏🏼👏🏼

<br/>
<br/>

# 🎨 docdash: JSDoc 템플릿 사용하기

## docdash 설치

JSDoc에 다양한 템플릿이 있는데, 그 중 `docdash` 가 많이 쓰인다고 한다.

docdash 를 사용하면 JSDoc의 가독성을 조금 더 높일 수 있다. (+ 심미적인 만족감)

```bash
npm install docdash
```

<br/>

## jsdoc.config.json 수정

jsdoc.config.json 을 수정한다.

```json
"opts": {
	"template": "node_modules/docdash",
	...
}
```

<br/>

다시 아래 명령어를 입력해서 JSDoc을 생성하면 아래와 같은 템플릿이 적용된다.

```bash
./node_modules/.bin/jsdoc -c jsdoc.config.json .
```

<img src='https://velog.velcdn.com/images/hailieejkim/post/02416de3-0658-4f6b-8ea5-8fab92b317ad/image.png' width=700px>

훨씬 읽기가 좋아졌다! 👍

<br/>
<br/>

# References

[jsdoc.app](https://jsdoc.app/)

[JSDoc 3.6 (한국어)](https://runebook.dev/ko/docs/jsdoc/-index-)

[jsdoc-boilerplate](https://github.com/pocojang/jsdoc-boilerplate)

[JSDoc을 사용해 JavaScript 파일 문서화하기](https://velog.io/@yijaee/JSDoc%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%B4-JavaScript-%ED%8C%8C%EC%9D%BC-%EB%AC%B8%EC%84%9C%ED%99%94%ED%95%98%EA%B8%B0)
