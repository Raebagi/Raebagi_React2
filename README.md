# 202030111 박래현

# 24-10-30 강의내용 정리

### 4.2 데이터 불러오기
- Next는 클라이언트와 서버 모두에서 데이터를 불러올 수 있음
- 서버는 다음 두가지의 상황에서 데이터를 불러올 수 있음
1) 정적 페이지를 만들 때 getStaticProps 함수를 사용해, 빌드 시점에 데이터를 불러올 수 있음
2) 서버가 페이지를 렌더링할 때 getServerSideProps를 통해, 실행 도중 데이터를 불러올 수도 있음.
- 데이터베이스에서 데이터를 가져올 수 도 있지만 안전하지 않기 때문에 권장하지 않음 데이터베이스 접근은 백엔드에서 처리하는게 좋음
- Next는 프런트엔드만 담당하는 것이 좋음.

### 서버가 데이터 불러오기
- 서버에서는 두 가지 방법으로 HTTP 요청을 만들고 처리할 수 있음
1) Node의 내장 HTTP 라이브러리를 사용할 수 있음. 다만 서드파티 HTTP클라이언트와 비교했을 때 설정하고 처리해야 할 작업이 더 많은 편입니다.
2) HTTP 클라이언트 라이브러리를 사용할 수 있음 ex- Axios

## Next.js REST API 사용

### 1. REST API란
### 2. Json Server
### 3. axios

#### 1. REST API - 개요

- REST(Representational State Transfer)란 자원을 이름으로 구분하여 그 자원의 상태를 통신을 통해 주고 받는 것

1) HTTP URI(Uniform Resource Identifier : 통일된 자원 식별자)를 이용해서 자원(Resource)을 명시함.
2) HTTP Method(POST, GET, PUT, DELETE, PATCH 등)를 통해 자원에 CRUD를 적용.

- CRUD란 데이터 처리의 기본적인 기능을 나타냄.

1. Create : 데이터 생성(POST)
2. Read : 데이터 조회(GET)
3. Update : 데이터 수정(PUT, PATCH)
4. Delete : 데이터 삭제(DELETE)

- REST API란 REST의 규칙을 적용한 API를 의미

#### REST API 설계 구칙

- URI는 동사보다는 명사를, 대문자보다는 소문자를 사용하여야 함. <br>
Example : https://develop.com/run
- 주소의 마지막에 슬래시 포함 x <br>
Example : https://develop.com/test
- 단어를 연결할 때는 하이픈(-) 사용 <br>
Example : https://develop.com/test-blog
- 파일 확장자는 URI에 포함 x <br>
Example : https://develop.com/photo
- URI에 메소드 포함 x <br>
Example : http://develop.com/post/1

#### Json Server

- BackEnd가 개발되기 전이나, 아직 외부 API가 결정되지 않았다면 local에 Json server를 구축하고 Frontend 개발을 하기에 적합한 node 패키지.
- 다음 명령으로 json-server를 설치
```js
$ npm i -g json-server

added 45 packages in 3s

14 packages are looking for funding
  run `npm fund` for details
```
- 설치가 잘 되었는지 버전 확인
```js
$ json-server --version
1.0.0-beta.3
```

#### 3. Axios 설치
```js
$ npm i axios

added 9 packages, and audited 322 packages in 2s

130 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
```

#### Axios 사용하기

- Axios Ex
```js
const res = await axios.get("https://api.example.com");
const products = res.data; // Axios에서 응답 본문의 데이터를 가져옴
```
- axios를 사용하여 객체를 생성하고는 다시 데이터를 저장함. 왜?

- Fetch API의 예시이다. 비교해보자
```js
fetch('https://api.exmapl.com')
.then(res => res.jso()) //then에서는 json()으로 데이터 추출
.then(data => {
    console.log(data); // 여기에 실제 응답 데이터가 있음
});
```
- axios에서 res객체는 통신에 필요한 데이터까지도 포함.
- 따라서 res.data를 이용해서 json데이터만 추출하는 것

#### Axios 사용하기

- axios.get()을 통해 받아온 응답 객체인 res는 단순히 JSON 데이터만 담고있는게 아니라, HTTP 통신 관련된 여러 정보들을 함께 포함하고 있음.<br>
예를 들어
- res.status : HTTP 응답 상태 코드(200,404,500 등)
- res.headers : 서버로부터 받은 헤더 정보
- res.config : 요청에 대한 설정 정보
- res.statusText : 응답 상태에 대한 설명 (예 : "OK")
- res.data : 서버가 실제로 전송한 데이터
- res는 실제로 서버가 전송한 데이터이다.

#### Axios 사용하기

- 작성한 data.json으로부터 정보를 받아와보자

#### Axios 사용하기

- 그런데 위와 코드는 비동기 데이터 로딩과 상태 관리가 제대로 고려되지 않았기 떄문에 몇 가지 문제가 있을 수 있음.
- 특히 Next.js와 같은 링낵트 기반 앱에서 비동기 데이터를 처리할 때 렌더링 주기에 맞게 상태를 관리해야함.

#### [개선할 부분]

1. useState와 useEffect 사용.
- 비동기 데이터를 가져오는 작업은 컴포넌트의 상태(state)로 관리하는 것이 일반적임. 현재 코드에서는 users 데이터가 비동기적으로 로드되는데, 이를 관리하기 위한 useState와 useEffect 훅이 빠져있음.
- 데이터를 로드하기 전에 컴포넌트가 렌더링되기 떄문에, users 변수가 초기에는 존재하지 않아 undefined 에러가 발생할 가능성이 있음

2. Loading 상태 처리
- 데이터를 불러오는 동안 사용자가 기다릴 수 있도록 로딩 상태를 추가하는 것이 좋음.

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