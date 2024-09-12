# 24-09-04 2주차 강의

#### 프로젝트 생성 방법

- 만일 개발환경에 yarn 패키지가 설치되어 있다면 Next.js는 yarn을 사용하여 프로젝트를 초기화함.
- npm을 기본으로 사용하고 싶다면 다음 명령으로 설정을 덮어씀
```js
npx create-nextt-app <app-name> --use-npm
```
- 또다른 프로젝트 생성 방법으로는 GitHub저장소에서 원하는 보일러플레이트 코드를 다운로드해 새 Next.js 프로젝트를 시작할 수 있음
```js
npx create-next-app --example <보일러플레이트 이름>
npx create-next-app --eaxmple blog-starter
```

#### 프로젝트의 기본 구조

- pages/디렉토리 안의 모든 js파일은 public 페이지가 됨
- pages/ 의 index.js 파일을 복사해서, about.js 로 이름을 바꾸면 http://localhost:3000/about로 접속 가능
- public/ 디렉토리에는 웹사이트의 모든 퍼블릭 페이지와 정적 콘텐츠가 있음
- styles/ 디렉토리에는 앱에서 사용하는 스타일 시트를 넣음
- 용도가 정해져 있는 디렉토리는 pages/ 와 public/ 뿐임
- 나머지 디렉토리는 필요에 따라서 다른 목적으로 사용하거나 삭제해도 됨.

#### Next.js 14의 프로젝트의 기본구조

- 프로젝트를 생설할 때 /src 사용 여부를 선택할 수 있고, 일반적으로 사용됨
- 14에서는 /public과 /src/app 디렉토리만 용도가 정해져 있음.

#### 타입스크립트 지원

- Next.js는 타입스크립트로 작성되었기 떄문에 고품질의 type definition을 지원.
- 기본 언어를 타입스크립트로 지정하려면 root에 tsconfig.json 이라는 설정 파일을 생성
- 그런 다음 npm run dev 명령을 실행하면 다음과 같은 메시지 확인 가능
- Please install typescript, @types/react, and @types/ndoe by running ~~
- npm install --save-dev typescript @types/react @types/node
- 설정을 임의로 변경하는 경우엔 몇가지 주의사항이 있음.

#### 바벨과 웹팩 설정 커스터마이징

- 바벨이나 웹팩의 설정도 커스터마이징 할 수 있음
- 바벨은 자바스크립트 트랜스컴파일러이며, 최산 자바스크립트 코드를 하위 호환성 보장하는 스크립트 코드로 변환하는 일을 담당.
- 하위 호환성이 보장되면 어떤 웹 브라우저에서든 자바스크립트 코드를 실행 가능
- 바벨을 사용하면 브라우저나 node.js 등에서 지원하지 않는 새롭고 훌륭한 기능을 현재의 환경에서도 실행 가능
- node.js에서 실행하면 오류가 나지만, 바벨을 사용하면 실행가능한 ECMAScript코드로 바꿔줄 수 있음.
- 바벨 설정을 커스터마이징 하려면, 프로젝트 Root에 .babelrc라는 파일을 생성하면 됨.
- 이 설정 파일을 비워두면 오류가 발생하기 때문에 최소한 다음 내용을 저장해야 함.

#### ECMAScript 기능 중 파이프라인 연산자를 사용해보자.

- 파이프라인은 공식적으로 채택되지 않은 연산자임.
```js
console.log(Math.random() * 10);
// 파이프라인 연산자를 사용하면 위 코드를 아래와 같이 바꿀 수 있음
Math.random()
|> x => x * 10
|> console.log;
```

- 기능을 사용하려면 바벨 플러그인을 설치해야함
```js
npm install --save-dev @babel/plugin-proposal-pipeline-operator @babel/core
```
- 그리고 .babelrc 파일을 다음과 같이 수정함.
```js
{
    "presets": ["next/babel"],
    "plugins": [
        [
            "@babel/plugin-proposal-pipeline-operator",
            {"proposal" : "fsharp"}
        ]
    ]
}
```
- 이제 개발 서버를 재시작하면 됨.