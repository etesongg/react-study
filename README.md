# react-study
# 리액트 스터디 4주 활동

### 1. 리액트 기초
<details>
<summary>리액트란?</summary>

### 1. 리액트란? : 불편함을 해결하자!

불편한 점: 

1. document.getElementById(””)
2. 자바스크립트와 HTML파일이 따로놈( 관리가 힘듬)
3. 새 페이지 들어갈때마다 새로고침이 됨

리액트:

1. document.getElementById(””) 쓰지 않음
2. JSX를 사용해 관련있는 HTML과 JS 합쳐주기
3. 새로운 페이지, 메뉴 들어갈때 새로고침 할 필요 없음
4. 코드의 재사용이 높음(컴포넌트 사용)

### 2. 환경설정: Node.js를 설치하자!

리액트에 필요한 패키지를 자동으로 설치해주는 명령어 입력 : ***npx create-react-app [폴더명]***

ex) npx create-react-app in-react

- 리액트에서 html 파일은 딱 하나다.
    - SPA: Single Page Application
- 프로젝트는 터미널에 ***npm start***  시작할 수 있다.

### 3. component: 재활용의 시작

- App.js 파일에 하나의 태그 안에 모든 요소가 들어가 있어야 한다.
    - 에러: *Adjacent JSX elements must be wrapped in an enclosing tag. Did you want a JSX fragment <>...</>?*
    - 원인: React에서는 JSX를 사용할 때 여러 개의 요소를 반환하려면 하나의 부모 요소로 감싸지 않았기 때문.
    - 해결: div 태그를 사용하여 모든 요소를 감싸기.
- 리액트 컴포넌트 자동완성 단축어 ***rafce***
    - es7 react 확장 설치 시 사용가능

```jsx
function App() {
  return (
    <div>
      <div className="box">복숭아</div>
      <div className="box">복숭아</div>
    </div>
  );
}

컴포넌트 생성
import Box from "./component/Box";

function App() {
  return (
    <div>
      <Box />
      <Box />
    </div>
  );
}
```

- 컴포넌트 생성시 주의사항: 컴포넌트의 이름은 반드시 대문자로 시작해야 한다.
    - 리액트가 컴포넌트와 일반 HTML 태그를 구별하는 방법은 태그가 대문자로 시작하는지 여부이다. 그렇기 때문에 반드시 첫글자는 대문자로 만들자!
- 어떤걸 컴포넌트로 만들어야 할까?
    1. 반복 되는 부분.
        1. 리스트의 아이템, 반복되는 UI 요소는 컴포넌트로 만드는게 좋다.
    2. 기능별로.
        1. 개발할때나 테스트, 유지보수할 때 용이하게 끔 기능별로 나누는게 좋다.
    3. 하나에 한 기능만
        1. 나중에 재활용 될 경우를 대비해 한 컴포너트에 한 기능만 들어있는 것이 좋다. 

### 4. props: 함수의 매개변수와 같은 존재

```jsx
App.js
function App() {
  return (
    <div>
      <Box name="복숭아" num={1}/>
      <Box name="자두" num={2}/>
      <Box name="수박" num={3}/>
    </div>
  );
}

Box.js
const box = (props) => {
    console.log("props",props)
  return (
    <div className="box">
      Box{props.num}
      <p>{props.name}</p>
    </div>
  )
}
```

- 콘솔창에 결과가 두번씩 나타나는 문제를 해결하기 위해 index.js의 <React.StrictMode>태그를 주석 처리함.

### 5. state: 리액트가 리액트인 이유

- state (값이 업데이트 되면 자동으로 ui도 업데이트 시켜주자
    - 변수값이 업데이트 된건 신경쓰지 않지만 state 값이 업데이트 됐으면 업데이트 해줌
- setCounter2() 함수를 이용해서만 counter2값을 바꿀 수 있음
- UI에 보여줘야 하는 값은 state로 만들어야 함
- 변수는 re render 될때마다 초기화가 된다.
- state는  한박자 느림
    - ui를 업데이트 하는건 비쌈, 그렇기 때문에 state가 있어도 해당 함수가 끝날때까지 기다렸다가 함수가 끝나면 처리해줌 즉 **비동기 적임** 그렇기 때문에 위에 콘솔에 있는 state count2는 카운트가 1씩 늦는것이다.
- 왜 count 는 영원히 1인가
    - state값을 업데이트 할때마다 function App() 다시 실행시켜주면서 새로운 값으로 ui를 업데이트 됨. function App()(컴포넌트)를 다시 실행시켜주기 때문에 count 값이 0으로 계속 초기화 됨(state같은 경우에는 그 전의 값을 기억해둠)

- 그렇기 때문에 리액트에서 변수는 잠깐 저장해 두는 값으로 쓰고 나머지는 state로 사용

```jsx
App.js
import { useState } from "react"; // react 자체에서 제공해주는 useState 불러오기

function App() {
  let counter = 0;
  //useState는 배열을 반환함, [변수, 함수]와 useState(기본값) 형식으로 사용
  const [counter2, setCounter2] = useState(0);
  const increase = () => {
    counter += 1;
    setCounter2(counter2 + 1); // setCounter2() 함수를 이용해서만 counter2값을 바꿀 수 있음
    console.log("counter", counter, "counter2", counter2)
  };
  
  return (
    <div>
      <div>counter: {counter}</div> {/* counter는 1로 고정 */}
      <div>stateCounter: {counter2}</div> {/* state 한박자씩 늦음 */}
      <button onClick={increase}>클릭</button>
    </div>
  );
}
```

> 📌 state의 값은 계속 바뀌는데 왜 const로 선언 할까?
> 
> 
> > state는 일반 변수와 달리 setState 함수를 이용하여 값을 변경한다. (**함수 내부의 변수**가 함수 수명이 끝나더라도, 변수의 참조가 계속 된다면 그 **변수의 수명은 계속 된다. (**이걸 클로저라고함) 이렇게 우린 컴포넌트가 render 가 되더라도 state를 기억할 수 있다.)
> > 
> 
> 하지만 let을 사용하게되면
> 
> counter2=100 과같은 변수형식의 할당이 가능해진다.
> 
> 따라서 재할당을 방지하고 setState를 사용한 변수 변경만 허락하기위해 const로 선언한다.
>
</details>

### 2. 첫번째 프로젝트 - 가위바위보 게임
<details>
<summary>가드 값 넣어주기</summary>
1. 박스 2개 (타이틀, 사진, 결과로 구성)
2. 가위 바위 보 버튼
3. 버튼을 클릭하면 클릭한 값이 박스에 보인다.
4. 컴퓨터는 랜덤하게 아이템 선택이 된다.
5. 3 4 의 결과를 가지고 누가 이겼는지 승패를 따진다.
6. 승패결과에 따라 테두리 색이 바뀐다. (승-초록, 패-빨강, 비김- 검정)

- 함수 콜백형태
    
    리액트는 실행될때 ui를 그림
    그때 함수를 바로 실행시켜버림(이벤트가 실행되지 않았음에도 불구하고)
    그래서 콜백함수 형태로 함수를 전달해줘야 함
    

```jsx
<button onClick={() => play("scissors")}>가위</button>
```

- 상황에 맞게 가드값을 넣어줘야 함
    - 조건부 랜더링
        - if-else
        - 논리 연산자 &&
        - 삼항연산자

```jsx
src={props.item && props.item.img}
```

### 넷리파이 배포하기

- Build command에 CI=false npm run build 작성하기
</details>

### 3. 클래스 컴포넌트
<details>
<summary>constructor(props){ super(props) </summary>
- 컴포넌트는 함수 컴포넌트와 클래스 컴포넌트 두가지가 있음
- 좀 더 간결한 함수 컴포넌트가 나왔음에도 클래스 컴포넌트가 더 많이 사용됨
    - 많은 개발 문서들이 class 컴포넌트로 작성 됐기 때문, 그리고 클래스 컴포넌트만 되는 기능들이 있었음
        - 하지만 지금은 함수 컴포넌트로 안 되는 기능 없음. 또한 리액트 공식 문서에서 함수 컴포넌트를 사용할 것을 더 추천 함.

- 클래스 컴포넌트 자동완성 단축어 ***rcc***
- **constructor(props){ 
        super(props) …** 이 기본 구조 안에 state를 설정할 수 있음
- state를 부를 때에는 **this.state**를 꼭 붙여야 한다.
- 함수 앞에 const를 붙이지 않는다.
- state 값을 변화 시킬때는 **this.setState()** 함수를 사용한다.
    - **함수 안에는 오브젝트{}**로 써준다.
- index.js 에서 AppClass 파일 불러오도록 수정
- BoxClass.js 생성시 값을 보여주기 위해서는 **this.props** 사용해야 함.

```jsx
AppClass.js
constructor(props){ // 생성자: 컴포넌트가 실행되자 마자 호출 되는 함수
        super(props)
        this.state = { // 한번에  변수 여러개 정의 가능 
            counter2: 0,
            num: 1,
            value: 0
        };
    }
 increase = () => {
        this.setState({ // 한번에  변수 여러개 조작 가능 
            counter2: this.counter2 + 1,
            value: this.value + 1,
        });
    }
    
    render() {
    return (
      <div>
        <div>state: {this.state.counter2}</div>
        <button onClick={this.increase}>클릭</button> 
        <BoxClass num={this.state.value} />
      </div>
    )
  }
  
  BoxClass.js
    render() {
    return <div>Box {this.props.num}</div>;
  }
```
</details>

### 4. 리액트 LifeCycle
<details>
<summary>클래스 컴포넌트 lifecycle + 함수형</summary>

![라이프사이클](https://github.com/user-attachments/assets/753dd6cc-440d-4be0-91a5-fbb3c6fbf1c1)

- Mounting: 컴포넌트가 시작될때
- Updating: state가 업데이터 되고 UI 업데이트 될때
- Unmounting: 컴포넌트가 종료될때

- constructor: lifecycle에서 첫번째로 실행되는 함수. 컴포넌트가 실행될때 먼저 호출하고 들어감. 그러므로 앱이 실행되자 마자 해줘야 하는 작업들을 constructor에 넣음.
- render: UI 그려주는 부분
- componentDidMount: UI 세팅이 완료되면 알려줌, 여기에서 api 콜 작업을 함.
- componentDidUpdate: state가 업데이트 되면 알려줌. 최신 업데이트된 state 값을 받아볼 수 있음, 최신 없데이트 된 값을 가지고 무언가 해야할때 여기서 작업함.
- componentWillUnmount: 컴포넌트가 종료될때 실행.

### 함수형 컴포넌트 lifecycle

- useEffect()는 매개변수를 2개를 받는다. (콜백함수와 배열)
- 배열에 들어가있는 값중에 하나라도 state 값이 변하면 useEffect가 호출이 된다.
    - 두개의 값이 동시에 바뀌어도 한번만 실행된다.

```jsx
import { useEffect } from "react";

function App() {
...
  useEffect(()=>{ // 배열에 아무것도 없으면 componentDidMount() 처럼 작동
    console.log("useEffect1")
  }, [])

  useEffect(()=>{ // 배열 안에 state가 있으면  componentDidUpdate() 처럼 작동
    console.log("useEffect2 value",value )
  },[counter2, value]) // 배열에 여러개 값 넣을 수 있으며 state에 따라 독립적으로 useEffect를 사용해도 된다. 
```
</details>

### 5. 두번째 프로젝트 - 날씨앱
<details>
<summary>App 이 필요한 모든 함수와 state를 가지고 있음</summary>
```jsx
function WeatherBox({weather}) { // props 대신 Destructuring 사용 키값으로만 불러옴
  return (
    <div className="weather-box">
      <div>{weather?.name}</div> {/* weather 참이면 name을 보여줘 */}
      <h2>{weather?.main.temp}°C / {Math.ceil((weather?.main.temp*1.8+32)*100)/100}°F</h2>
      <h3>{weather?.weather[0].description}</h3>
    </div>
  );
}
```

- 섭씨를 화씨로 변환하기
    - 화씨 = 섭씨*1.8+32
    - 소수점 두번째 자리까지 보여주기  Math.ceil(number * 100) / 100;
    - 올림 함수 Math.ceil()
    - 해당 숫자에 *100후 100으로 나눈 후 올림하면 소수 두번째 자리까지 보임

- 리액트의 단점 중 하나는 부모가 자식한테 값을 주는건 되지만 자식이 부모한테 값을 넘기지는 못함
    - App 이 필요한 모든 함수와 state를 가지고 있음
    - 그렇기 때문에 App에서 모든걸 만들어서 props로 보내줌(함수도 보낼 수 있음)

```jsx
  useEffect(() => {
    if (city == "") {
      getCurrentLocation();
    } else {
      getWeatherByCity();
    }
  }, [city]);
```

- 초기에 city가 비어있는 상태에서 useEffect( getWeatherByCity())를 실행하면 에러남, 버튼을 통해 도시를 선택한 경우 정보를 얻을 수 있는 함수이기 때문
- 그렇기때문에 useEffect를 하나로 합친 후 조건문으로 해결해줌
</details>

### 6. 멀티 웹페이지(라우터)
<details>
<summary>RESTful Routes</summary>
### 1. 리액트 라우터 기초

```jsx
https://reactrouter.com/en/main/router-components/browser-router
1. 리액트 라우터 설치
**npm install react-router-dom@6**

2. index.js 추가
import { BrowserRouter } from "react-router-dom";
<BrowserRouter>
    <App />
</BrowserRouter>
  
3. App.js 추가
import { Routes, Route } from "react-router-dom";
<Routes>
   <Route path="/" element={<Homepage />} />
   <Route path="/about" element={<Aboutpage />} />
</Routes>

**라우터 기본형식
<Route path="/" element={} />** 
```

### 2. Link, Navigate 페이지 사이를 이동하는 법

```jsx
1. Link는 a 태그 형식과 비슷, 사용법
import { Link } from 'react-router-dom'
**<Link to="/about">go to aboutpage</Link>**

2. Navigate는 버튼에 onclick 함수로 보낼때 사용, 사용법
import { useNavigate } from 'react-router-dom'
const Aboutpage = () => {
    **const navigate = useNavigate()
    
    const goToHomepage = () => {
        navigate("/") // 내가 가고싶은 주소
    }**
  return (
    <div>
      <p>aboutpage</p>
      **<button onClick={goToHomepage}>go to homepage</button>**
    </div>
  )
}
```

### 3. Restful Route(UI 디자인 패턴)

```jsx
1. 사용법
<Route path="/product" element={<Productpage />} />
<Route path=**"/product/:id"** element={<ProductDetailpage />} /> // /product/123 형식으로 접속가능

2. HTTP명령어
Get : 데이터를 가져올 때 쓰임. (fetch하면 기본 명령어 속성이 Get임)
Post : 데이터를 생성할 때 쓰임.
Put : 데이터를 수정할 때 쓰임.(Patch 라고도 불림)
Delete: 데이터를 삭제할때 쓰임.

3. Restful Route의 필요성
우리가 쇼핑몰 아이템을 보여주는 페이지가 있다고 가정하자
/getItem /createItem /updateItem /deleteItem
이런식으로 하면 이름에 통일성이 없어지고 
어떤 아이템에대해서 모든 생성,읽기,수정,삭제 행위에 대해서 총 4개의 url이 필요하다

이렇게 하면 url은 길고 복잡해진다. 이를 해결하기 위해 나온게 restful 디자인이다
url에서 동사는 빼고 이를 Http 명령어로 대체한다
따라서

/items + get 명령어 = 아이템읽어오기
/items + post 명령어 = 아이템 생성하기 
/items + put 명령어 = 아이템 수정하기 
/items + delete명령어 = 아이템 삭제하기
이런 규칙으로 바뀐다
즉 /items라는 url 하나로 4가지의 액션을 할 수 있게 되었다.

내가 하나의 아이템만 가져오고싶다면 뒤에 아이템의 id를 붙이는것도 restful route의 규칙이다

/items/:id +get 명령어 = id를 가진 아이템읽어오기 
/items/:id +put 명령어 = id를 가진 아이템 수정하기 
/items/:id +delete 명령어 = id를 가진 아이템 삭제하기
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/f664cccb-415a-476a-a59d-98876f7e9666/0c4553a7-6328-4284-b85d-c783b6744925/image.png)

### 4. useParams: URL의 파라미터값을 읽어오자

```jsx
1. 사용법
import { useParams } from 'react-router-dom'
const params = useParams(); // {id: '1'} 
// id 라는 키값은 Route path="/product/:id" 의 :id에서 왔고 
// 벨류값은 현재 접속한 페이지번호값이다.
// 만약 Route path="/product/:id/:num" 이라면 {id: 'aaa', num: '23'} 이런식으로 나온다.

**const {id} = useParams();** // Destructuring 
  return (
    <div>
      <p>Show Product Detail **{id}**</p> {/* 이런식으로 사용할 수 있다.*/}
    </div>
  )
```

### 5. useSearchParams: url 쿼리값을 읽어보자

```jsx
1. 쿼리란
  const navigate = useNavigate();
  const goProductpage = () => {
      navigate("/product?q=pants")
  }
<button onClick={goProductpage}>go to productpage</button>

/product?q=pants 페이지로 들어가보면 productpage가 잘 뜸
? 쿼리 뒤에 있는 값은 url 경로에 영향을 미치지 않기때문

2. 사용법
import { useSearchParams } from 'react-router-dom'
  **let [query, setQuery] = useSearchParams();**
  console.log(query.get("q")) // pants
  
  // 만약에 쿼리가 길때 ?q=skirt&num=2&page=3
 **console.log(query.get("page")) // 3**
```

### 6. Redirect: 페이지를 보호하는 법(가면 안되는 페이지)

```jsx
1. Navigate 컴포넌트 사용

2. 사용법
import { useState } from "react";
import { Routes, Route, **Navigate** } from "react-router-dom";

function App() {
  const [authenticate, setAuthenticate] = useState(false);
  **const PrivateRoute = () => { // 첫글자 대문자이기 때문에 컴포넌트임
    return authenticate == true? <Userpage />: <Navigate to="/login" />;
  }**
  return (
  <div>
    <Routes>
      <Route path="/" element={<Homepage />} />
      <Route path="/about" element={<Aboutpage />} />  
      <Route path="/product" element={<Productpage />} />
      <Route path="/product/:id" element={<ProductDetailpage />} />
      <Route path="/login" element={<Loginpage />} />
      **<Route path="/user" element={<PrivateRoute />} />**
    </Routes>
  </div>
  );
}
```
</details>

### 7. 세번째 프로젝트 - H&M 쇼핑몰 웹사이트
<details>
<summary>json server</summary>
- json server
    
    ```jsx
    루트폴더에 db.json 파일을 만들어 데이터를 넣어야 함
    {
        "products":[
            {
                "id":0,
                "img":"https://noona-hnm.netlify.app/pattern-jacket.jpeg",
                "title":"벨티드 트윌 코트",
                "price":99900,
                "choice":true,
                "new":true,
                "size":["S","M","L"]
    
            },
            {
                "id":1,
                
    시작 명령어 
    json-server --watch db.json --port 5000 // 포트번호 지정가능
    안될경우 앞에 npx 붙이고 실행
    ```
    

- <Form> 태그와 Button의 tyep=”submit”
    - <Form> 안에 Button의 tyep=”submit”이면 onClick으로 이벤트를 못 줌
    - onSubmit 이벤트는 줘야 함
    - 그리고 Form이 버튼 눌릴때마다 리프레시 되는걸 막아줘야 함
        - Form  자체에서 주는 이벤트가 있음 이걸 활용해서 막을 수 있음
        
        ```jsx
        즉 Form을 사용하면 항상 이걸 해줘야 함.
        리프레시 막아줌
        **const loginUser = (event) => {
            event.preventDefault();**
            console.log("login user function issue")
          **}**
          return (
            <Container>
              **<Form onSubmit={(event) => loginUser(event)}>**
        ```
        

- 리액트에서는 읽어오고 싶은값이 event에 들어가 있음(js에서는 .value)
    - let keyword = event.target.value;
</details>

### 8. 리덕스
<details>
<summary>redux 와 react-redux 설치</summary>
- 리액트가 쓸 수있는 라이브러리
-> 자식 컴포넌드끼리 state를 전달할 수 없는 특징을 가진 리액트는 공통 부모(App)가 state를 생성하고 props로 자식들에게 전달함(리액트는 단방향 소통을 함) 이때 props가 너무 많아져 관리하기 힘들다는 문제가 생겨 리덕스가 생김.
리덕스는 store라는 저장소를 만들어 언제든지 접근할 수 있게 함
store는 객체타입
하지만 컴포넌트가 store의 값을 바로 바꾸거나 요청하지 못 함

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/f664cccb-415a-476a-a59d-98876f7e9666/ea47af85-dc71-44c2-8e9f-da9fb4afb805/image.png)

- Action: 로그인하기 같은 행동을 받음
- Reducer: 가지고 있는 작업지침 중 해당하는 걸 실행함
    - case "로그인하기"
    return {id: userId, password: userPassword, authenticate: true}
    - 유저 id, password 를 저장하라고 되어 있군, authenticate를 true 로 바꾸라고 되어 있군
    - 이후 Reducer가 행동지시에 따라서 Store의 값을 바꿈
    - Store의 값이 바뀌면 자동으로 component 바뀌면서 re render 해줌

여기서 알아야 하는 리액트 훅
useDispatch: 액션을 던지는 훅(component →Action 사이)
useSelector: Store에 있는 값을 가져다 쓸 때 사용하는 훅(Store → Component 사이)

### redux 셋업

```jsx
redux 와 react-redux 설치
npm install redux react-redux 

index.js
import { Provider } from 'react-redux';

root.render(
  <Provider store={store}>
    <App />
  </Provider>
);

src 폴더 안에 redux 폴더
redux 폴더 안에 store.js, reducer 폴더 
reducer 폴더 안에 reducer.js
(이때 reducer를 폴더로 만드는 이유는 기능에 따라 여러 파일로 만들어 줄 수 있기 때문)

reducer는 두개의 매개변수를 받음 (state, action)
state는 초기화가 필요함(어떤 state가 있는지 알기위해)

let initialState = {
  count: 0
};

function reducer(state = initialState, action) {

}
```

### redux  적용

```jsx
1. useDispatch 액션 던져주기, increase란 버튼을 눌렀을때! (그때 값이 바뀌어야 하니까)
action을 던져야 하는데 action은 어떻게 생겼나 룰이 있는 그냥 단순한 객체
반드시 type이라는 키와 선택사항인 payload라는 키가 있어야 함

import { useDispatch } from 'react-redux'

function App() {
  const dispatch = useDispatch();
  
    const increase = () => {
    dispatch({type:"INCREMENT"})
  }

2. reducer는 자동으로 dispatch가 던진 액션을 받아올 수 있음
3. Store는 return으로 store의 값을 바꿈
새로운 객체를 전달받아야 본인이 바뀐걸 앎 
return 할때 ...state는 기본적으로 쳐야 한다고 생각해야함. 

function reducer(state=initialState, action){
    console.log("action", action);
    if(action.type === "INCREMENT") { // 회사에 따라 switch 문으로 하기도 함
        return {...state, count: state.count + 1}; // ...state를 하는 이유: 
    }             // 만약에 state가 여러개면 다른 state값은 유지하되 count만 바꾼다
								   // ...spread 문법을 통하여 기존 객체내용을 복사해 새로운 객체에 전달 가능
    return { ...state } // 기본리턴 필요(위에 조건문에 안맞을때 리턴해줄게 필요하니까
}

switch 문인 경우
    switch(action.type) {
        case "INCREMENT":
            return {...state, count: state.count + 1};
        default:
            return {...state}
    }
   
   
4. Store 사용하기
이제 App에서 더이상 state는 만들지 않음

import { useSelector, useDispatch } from 'react-redux'

function App() {
  const count = useSelector(state=>state.count)
  const dispatch = useDispatch()

  const increase = () => {
    dispatch({type:"INCREMENT"})
  }
  return <div>
    <h1>{count}</h1>
    <button onClick={increase}>증가</button>
  </div>;
}

- 리덕스가 훌륭한 이유는?
    - 내가 부모건 자식이건 상관없이 어디서든 필요하면  state를 가져다 쓸 수 있다.
5. componet에서 사용하기
이제 App에서 props로 넘길 필요가 없다. 내가 직접 가져다 쓰면 됨
Box.js
import React from 'react'
import { useSelector } from 'react-redux' // App.js에서 사용할때랑 똑같이 사용

const Box = () => {
    let count = useSelector(state=>state.count)
  return (
    <div>
      this is box {count}
    </div>
  )
}

export default Box

컴포넌트에서 컴포넌트를 사용한다고 해도 사용하는 방법은 똑같음

 6. dispatch에서 payload는 매개변수 같은 개념으로 내가 원하는 정보를 보내줄 수 있음
 
 App.js
<button onClick={login}>Login</button>
       
const login = () => {
    dispatch({ type: "LOGIN", payload: { id: "song", password: "123" } });
  };

reducer.js  
  let initialState = {
  count: 0,
  id: "",
  password: "",
};

// 조건 추가
else if (action.type === "LOGIN") {
    return {
      ...state,
      id: action.payload.id,
      password: action.payload.password
    };
```
</details>

### 9. 네번째 프로젝트 - 연락처 페이지(리덕스 사용)
<details>
<summary></summary>

- button 의 type="submit"인 경우 주의!!
onclick이 아닌 onsubmit으로 해야하며 버튼을 감싸고 있는 Form에 이벤트를 추가해줘야 함
그리고 submit은 새로고침을 하기 때문에 event.preventDefault();를 추가해 새로고침 안되게 해야함.
</details>

### 10. 리덕스 미들웨어
<details>
<summary>리덕스는 비동기 작업을 할 수 없음</summary>
리덕스의 단점

- 비동기 작업을 할 수 없음 → 미들웨어를 통해 해결
- 즉 async 함수들을 미들웨어에서 다뤄주면 됨

리덕스는

- 동기적으로 state처리
- 미들웨어는 함수를 리턴, 이 함수는 2개의 매개변수가 있는데 dispatch, getState
- 리듀서를 각각 나눠서 파일을 만들었다면 index.js 파일을 만들어 객체 형태로 합쳐서 전달해야 한다. → 객체 형태이기 때문에 useSelect해줄때 state.키값을 넣어줘야 한다.

데이터가 사용되는 페이지 → dispatch로 함수를 호출해 액션에 있는 함수로 보냄 → 액션에서 정의해놓은 함수에서 받아 dispatch로 액션을 던진다 → 리듀서로 받아 행동지침을 따른다

- 리덕스 유용한 툴
    - redux devtools 크롬 다운

---

리덕스 툴킷

- 리덕스를 사용할때
- reducer 에서 스위치 케이스 문으로 만들어서 액션마다 이름도 지어줘야 하고 귀찮…, 리턴값에 ..state 중복으로 쓰는것도 싫음
    - 리덕스 툴킷은 createSlice가 알아서 유니크한 네임을 만들어 줄거임?
    - createSlice가 꼭 필요해하는 값 3가지(name, initialState, reducers)
- store 에서 지금까지 만든 리듀서들을 컴바인해서 알려주고, thunk, applyMiddleware, composeWithDevTools를 항상 해줘야 했는데 이걸 생략할 수 있게 해줌


-npm install @reduxjs/toolkit 리덕스 툴킷을 깔으면 리덕스도 같이 깔림
</details>

### 11. 리액트 쿼리
<details>
<summary>리액트 쿼리는 데이터를 캐시에 저장함</summary>
- **npm i @tanstack/react-query**
- **npm i @tanstack/react-query-devtools** 설치
- jsonserver 포트넘버 지정해서 실행하기
    
    json-server --watch db.json --port 3004
    
- **npm i axios**
    
    ```jsx
    객체 하나만 받음
    import { useQuery } from '@tanstack/react-query'
    ```
    

- 리액트 쿼리는 데이터를 캐시에 저장함
    - 나중에 다시 호출할때 같은 데이터를 부른다고 하면 바로 보여줄 수 있음.
    - 그 데이터가 아니더라도 미리 보여주고 fetch를 하기 때문에 로딩을 안 보여줘 유저 경험적으로 좋음
    - api 호출은 안 하는것은 아님 부르면 항상 호출 하지만 캐시를 먼저 보여주고 뒤에서 호출 됨
- api 호출 방법
    - 데이터가 fresh에서 stale로 변하는 시간 10초, 리액트 쿼리의 기본 staletime은 0임 그래서 캐시에 데이터가 들어갔더라도 뒤에서는 매먼 호출이 일어나는 것, 내가 가져오는 데이터가 해당 시간동안은 바뀌지 않을거라는 확신이 있을때 사용

fetch: api 호출되고 있을때

fresh: 데이터 도착했을때

stale: 도착한 후 시간이 지나면 스테일 // 리액트 쿼리에서는 이렇게 데이터 상태를 두가지로 구분함

fresh에서 stale로 가야지만 다시 api를 호출함(fetch가 일어남)(방금 받아온 데이터를 다시 받아오는건 낭비이기때문)
</details>

### 12. 다섯번째 프로젝트 - 넷플릭스 만들기
<details>
<summary></summary>

</details>
