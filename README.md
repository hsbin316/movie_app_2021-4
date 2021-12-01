# 허성빈 201840235
## [ 12월 01일 ] 
### <b>학습내용</b>
<b>React [주요개념](https://ko.reactjs.org/docs/getting-started.html)</b>  

4. Component와 Props [★](https://ko.reactjs.org/docs/components-and-props.html)
     - 컴포넌트 추출 : 컴포넌트를 여러 개의 작은 컴포넌트로 나누어 관리 편의성 및 가독성을 높인다.
          - props의 이름은 사용될 context가 아닌 컴포넌트 자체의 관점에서 짓는 것을 권장한다. 
          -  컴포넌트를 추출하는 작업이 지루해 보일 수 있다. 
          - 하지만 재사용 가능한 컴포넌트를 만들어 놓는 것은 더 큰 앱에서 작업할 때 두각을 나타낸다.
     - props는 읽기 전용이다.
          - 함수 컴포넌트나 클래스 컴포넌트 모두 컴포넌트의 자체 `props를 수정해서는 안된다`.
          - React는 매우 유연하지만 한 가지 엄격한 규칙이 있다. React 컴포넌트는 자신의 props를 다룰 때 반드시 `순수 함수`처럼 동작해야 한다.
               - [순수 함수](https://en.wikipedia.org/wiki/Pure_function)는 입력값을 바꾸려 하지 않고 항상 동일한 입력값에 대해 동일한 결과를 반환한다.
5. State and Lifecyle [★](https://ko.reactjs.org/docs/state-and-lifecycle.html)
     - Clock 컴포넌트를 완전히 재사용하고 캡슐화하는 방법을 알아본다.
     - [첫 번째 예제](https://codepen.io/gaearon/pen/gwoJZk?editors=0010)와 [두 번째 예제](https://codepen.io/gaearon/pen/dpdoYR?editors=0010)는 컴포넌트의 분리만 있을 뿐 차이가 없다.
     - 중요한 것은 clock이 스스로 업데이트하도록 만들기 위해서는 clock에 state가 있어야 한다.
     - `State`는 props와 유사하지만, 비공개이며 `컴포넌트에 의해 완전히 제어`된다.
     - 함수에서 클래스로 변환하기
          1. React.Component를 확장하는 동일한 이름의 ES6 class를 생성한다.
          2. render() 라고 불리는 빈 메서드를 추가한다.
          3. 함수의 내용을 redner()메서드 안으로 옮긴다.
          4. render() 내용 안에 있는  props를 this.props로 변경한다.
          5. 남아있는 빈 함수 선언을 삭제한다.
               - [결과물](https://codepen.io/gaearon/pen/zKRGpo?editors=0010)
     - 클래스에 로컬 State 추가하기 ( 예제 )
          1. render() 메서드 안에 있는 this.props.date를 this.state.date로 변경한다
               ```jsx
                class Clock extends React.Component {
                 render() {
                   return (
                     <div>
                       <h1>Hello, world!</h1>
                       <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
                     </div>
                    );
                  }
                }
               ```
          2. 초기 this.state를 지정하는 class constructor를 추가한다.
               - props를 기본 constructor에 전달하는지 유의
               ```jsx
                 class Clock extends React.Component {
                  constructor(props) {
                    super(props);
                    this.state = {date: new Date()};
                  }
                  render() {
                    return (
                      <div>
                       <h1>Hello, world!</h1>
                        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
                      </div>
                    );
                  }
                 }
               ```
          3. < Clock /> 요소에서 date prop을 삭제합니다.
               ```jsx
                ReactDOM.render(
                    <Clock />,
                    document.getElementById('root')
                );
               ```
               - [결과물](https://codepen.io/gaearon/pen/KgQpJd?editors=0010)
          - tick()메서드가 들어가지 않아 현재시간만 표시되고 시간에 따른 변화가 없다.
     - 생명주기 메서드를 클래스에 추가하기
          - Clock이 처음 DOM에 렌더링 될 때마다 타이머를 설정하려고 하는 것을 React에서 `마운팅`이라고 한다.
          - Clock에 의해 생섣된 DOM이 삭제될 때마다 타이머를 해제하려고 하는 것을 React에서 `언마운팅`이라고 한다.
          - componentDidMount() 메서드
               ``` jsx
                componentDidMount() {
                  this.timerID = setInterval(
                    () => this.tick(),
                    1000
                  );
                }
               ```
          - componentWillunmount() 메서드
               ```jsx
                componentWillUnmount() {
                  clearInterval(this.timerID);
                }
               ```
          - Clok 컴포넌트가 매초 작동할 수 있도록 tick()메서드를 구현하여 위 예제에 적용
               - [결과물](https://codepen.io/gaearon/pen/amqdNA?editors=0010)
          - 요약
               1. < Clock />가 ReactDOM.render()로 전달되었을 때 React는 Clock 컴포넌트의 `constructor`를 호출하여 `this.state를 초기화`한다.
               2. Clock 컴포넌트의 render() 메서드를 호출하여 화면에 표시되어야 할 내용을 알게 된 후 렌더링 출력값을 일치시키기 위해 DOM을 업데이트한다.
               3. `componentDidMount()` 생명주기 메서드를 호출하고 그 안에 Clock 컴포넌트는 매초 컴포넌트의 tick()메서드를 호출하기 위한 타이머를 설정하도록 브라우저에 요청한다.
               4. 매초 브라우저가 tick()메서드를 호출하고 그 안에 Clock컴포넌트는 setState()에 현재 시각을 포함하는 객체를 호출하여 `DOM을 업데이트`한다.
               5. Clock 컴포넌트가 DOM으로부터 한 번이라도 삭제된 적이 있다면 React는 타이머를 멈추기 위해 componentWillUnmount() 생명주기 메서드를 호출합니다.
     - State를 올바르게 사용하기
          1. 직접 State를 수정하지 않는다.
          2. State 업데이트는 비동기적일 수도 있다.
          3. State 업데이트는 병합됩니다.
     - 데이터는 아래로 흐른다.
          - 부모 컴포넌트나 자식 컴포넌트 모두 특정 컴포넌트가 stateful인지 또는 stateless 인지 알 수 없어 state는 종종 로컬 또는 캡슐화라고 불린다.
          - 컴포넌트는 자신의 state를 자식 컴포넌트에 props로 전달 할 수 있다.
          - 일반적으로 이를 `하향식` 또는 `단방향식` 데이터 흐름이라고 한다.
          - React 앱에서 컴포넌트가 `stateful` 또는 `stateless`에 대한 것은 시간이 지남에 따라 변경될 수 있는 구현 세부 사항으로 간주한다.
          - stateful 컴포넌트 안에서 stateless 컴포넌트를 사용할 수 있으며, 그 반대 경우도 마찬가지로 사용할 수 있다.
6. 이벤트 처리하기[★](https://ko.reactjs.org/docs/handling-events.html)
     - React 엘리먼트에서 이벤트를 처리하는 방식은 DOM 엘리먼트에서 이벤트를 처리하느 방식과 매우 유사하다.
     - 몇 가지 차이점
          - React의 이벤트는 소문자 대신 캐멀 케이스를 사용한다.
          - JSX를 사용하여 문자열이 아닌 함수로 이벤트 핸들러를 전달한다.
          - React에서는 false를 반환해도 기본 동작을 방지할 수 없으며, 반드시 preventDefault를 명시적으로 호출해야 한다.
          - JS에서는 DOM 엘리먼트가 생성된 후 리스너를 추가하기 위해 addEventListener를 호출하지만, React에서는 엘리먼트가 처음 렌더링 될 때 리스너를 제공하면 된다.
     - [예제](https://codepen.io/gaearon/pen/xEmzGg?editors=0010)

## [ 11월 24일 ]  
### <b>학습내용</b>
<b>create-react-app으로 [Remarkable] 사용하기</b>   
>1.npm create-react-app으로 markdown-editor 프로젝트를 생성한다.  
2.프로젝트가 정상 동작하는지 확인한다.   
3.App.js에 있는 필요없는 코드를 삭제한 후 문서의 코드를 복사해 넣는다.   
4.component의 이름을 App으로 수정한다.   
5.rendering은 index.js에 위임한다.   
6.App.js에서 React와 Remarkable을 import한다.   
7.프로젝트가 동작이 되는지 확인한다.   

<b>Markdown 예제</b>   
외부 컴포넌트를 사용하기 위해 생성자 내에 객체를 생성한다.

```jsx
class App extends React.Component {
  constructor(props) {
    super(props);
    this.md = new Remarkable();
    this.handleChange = this.handleChange.bind(this);
    this.state = { value: 'Hello, **world**!' };
  }
  ```
  state를 이용하여 Remarkable에 변환할 마크다운 문장을 제출한다.
  ```jsx
  handleChange(e) {
    this.setState({ value: e.target.value });
  }
  ```
  글이 입력되면 handleChange 이벤트를 사용하여 state의  value를 갱신하다.
  ```jsx
  getRawMarkup() {
    return { __html: this.md.render(this.state.value) };
  }
  ```
  getRawMarkup() 메소드를 통해 html을 반환 받는다.
  ```jsx
  render() {
    return (
      <div className="MarkdownEditor">
        <h3>Input</h3>
        <label htmlFor="markdown-content">
          Enter some markdown
        </label>
        <textarea
          id="markdown-content"
          onChange={this.handleChange}
          defaultValue={this.state.value}
        />
        <h3>Output</h3>
        <div
          className="content"
          dangerouslySetInnerHTML={this.getRawMarkup()}
        />
      </div>
    );
  }
}
```

<b>React 배우기</b>   
- React는 처음부터 점진적으로 적용할 수 있도록 서례되었으며 필요한 만큼 React를 사용할 수 있다.   
- 온라인 코드 편집기를 사용하여 간편하게 리액트를 경험할 수 있다.   
- CodeSandbox는 create-react-app으로 생성된 프로젝트와 동일한 환경에서 테스트가 가능하다.   
- CDN방식으로 간편하게 테스트를 할 수 있도록 HTML코드를 제공하고 있다.   
- React 문서가 어렵게 느껴진다면, [Tania Rascia가 쓴 React개요](https://www.taniarascia.com/getting-started-with-react/)를 먼저 학습하는 것이 도움이 된다.   
- 개발을 통해 React를 학습하고 싶다면 React홈페이지에 [자습서](https://ko.reactjs.org/tutorial/tutorial.html)를 통해 공부한다.   

<b>React [홈페이지](https://ko.reactjs.org/)</b>
> - <b>주요개념</b>   
개념을 단계별로 배우려면 `주요개념` 부터 시작하는 것을 추천   
> - <b>고급개념</b>   
강력하지만 일반적으로 많이 사용되지는 않는 React 기능을 소개   
> - <b>API참조</b>   
특정 React API를 자세히 알아보고 싶을 때 유용한 문서   
> - <b>Hook</b>   
16.8부터 새로 추가된 Hook에 대한 자세한 설명을 제공

<b>React [주요개념](https://ko.reactjs.org/docs/getting-started.html)</b>   
1. Hello World
     - 가장 단순한 React
     - CodePen에서 예제 코드를 실행해 보자
     - 지식수준 가정 읽어보기
2. JSX 소개
     - JSX에 표현식 포함하기
     - 함수의 호출 결과를 JSX에 표현식 포함하기
     - if,for문 등과 함께 사용, 변수에 할당, 인자로 받고, 함수로부터 반환할 수 있다.
     - 속성에 따옴표를 이용해 문자열 리터럴을 정의할 수 있다.
     - 속성에 중괄호를 사용하여 JacaScrtipt 표현식을 삽입할 수 있다.
     - 태그가 비어있다면 XML처럼 /> 를 이용해 바로 닫아주어야 한다.
     - Bable은 JSX를 React.createElement()호출로 컴파일 한다.
3. 엘리먼트 렌더링    
     - Element는 React앱의 가장 작은 단위이다.
     - React element를 root DOM 노드에 표시하려면 ReactDOM.render()로 전달하면 된다.
     - ReactDOM은 해당 엘리먼트와 그 자식 엘리먼트를 이전의 엘리먼트와 비교하고 DOM을 원하는 상태로 만드는데 필요한 경우에만 DOM을 업데이트한다.
     - CodePen에서 예제 코드들을 실행해 보자
4. Component와 Props
     - React에는 함수 컴포넌트와 클래스 컴포넌트가 있다.
     - 컴포넌트의 이름은 항상 대문자로 시작한다.
     - 문서'컴포넌트 렌더링'[예제](https://codepen.io/pen?&editors=0010&prefill_data_id=934e081c-1bdb-407b-8cde-2d41f527b896)의 실행 과정
          - Welcome name="Sara" /> 엘리먼트로 ReactDOM.render()를 호출합니다.
          - React는 {name: 'Sara'}를 props로 하여 Welcome 컴포넌트를 호출합니다.
          - Welcome 컴포넌트는 결과적으로 h1>Hello, Sara /h1> 엘리먼트를 반환합니다.
          - React DOM은 h1> Hello, Sara /h1> 엘리먼트와 일치하도록 DOM을 효율적으로 업데이트합니다.
     - 컴포넌트 합성: 컴포넌트는 출력에 다른 컴포넌트를 참조할 수 있다.


## [ 11월 17일 ]  
### <b>학습내용</b>
<b>상태를 가지는 컴포넌트</b>
 - 컴포넌트는 this.props를 이용해 입력 데이터를 다루는 것 외에도 내부적인 상태 데이터를 가질 수 있다. 이는 this.state로 접근할 수 있다.
 - 컴포넌트의 상태 데이터가 바뀌면 render()가 다시 호출되어 마크업이 갱신된다.

 <b>애플리케이션</b>
 - props와 state를 사용해서 간단한 Todo 애플리케이션을 만들 수 있다. 
 - 이벤트 핸들러들이 인라인으로 각각 존재하는 것처럼 보이지만, 실제로는 이벤트 위임을 
통해 하나로 구현된다.   

<b>Todo 예제</b>   
 Todoapp과 Todolist 두개의 컴포넌트로 구성   
 handleChange는 모든 키보드 입력마다 React의 state를 갱신해서 보여준다.
 > element에서 확인  

시간순으로 보면 다음과 같이 동작한다.    
 > 유저입력 > handleChange > React의 state 갱신 > form element가 React state를 참조
유저 입력을 강제로 대문자로 변경할 경우에도 사용한다.

```jsx
      class TodoApp extends React.Component {
           constructor(props){
                super(props);
                this.state = {items: [], text:''};
                this.handleChange = this.handleChange.bind(this);
                this.handleSubmit = this.handleSubmit.bind(this);
           }
           render() {
                return (
                     <div>
                         <h3>TODO</h3>
                         <TodoList items={this.state.items} />
                         <form onSubmit={this.handleSubmit}>
                              <label htmlFor="new-todo">What needs to be done?</label>
                              <input id="new-todo" onChange={this.handleChange} value={this.state.text}/>
                              <button>
                                   Add #{this.state.items.length + 1}
                              </button>
                         </form>
                     </div>
                )
           }
```
- render()메소드 에서 초기 렌더링을 실행한다.
- onChange를 통해 input에 입력되는 값으로 state 상태 변경을 준비한다.
- 입력된 값은 state의 "text: ' ' "에 임시로 저장된다.
- lavel의 htmlFor은 input과의 연결을 위한 id값이다
- 버튼을 클릭하면 버튼의 숫자를 증가시킨다.
- 리스트는 배열로 저장되기 때문에 item.length를 통해 list의 수를 확인한다.
- input area에 이벤트가 발생하면 handleChange(e)가 동작하여 State의 text값을 변경한다.
- 'Add #x'버튼을 클릭하면 리스트의 index에 1을 더해서 버튼에 출력한다.
```jsx
           handleChange(e) {
                this.setState({text: e.target.value});
           }

```
- 크롬 DevTool을 열어 실시간으로 state가 변화하는 것을 확인한다.
```jsx
           handleSubmit(e) {
                e.preventDefault();
                if(this.state.text.length === 0){
                     return;
                }
                const newItem = {
                     text: this.state.text,
                     id: Date.now()
                }
                this.setState(state => ({
                     items: state.items.concat(newItem),
                     text:''
                }))
           }
      }
```
- handleSubmit은 버튼이 클릭될 때 발생하는 event를 처리한다.   
> handleSubmit(e)에서 e.preventDefault()메소드를 사용하는 이유
> - 브라우저에서 양식을 제출할 때는 기본적으로 브라우저의 새로 고침이 발생하는데, React나 SPA의 경우 필요가 없는 동작임으로 이를 방지하기 위해 사용된다.

```jsx
      class TodoList extends React.Component {
           render() {
                return(
                     <ul>
                     {this.props.items.map(item => (<li key={item.id}>{item.text</li>}))}
                     </ul>
                )
           }
      }
```
- todolist class 생성한다
- ul안에 추가된 task를 li로 출력한다.
- 앞서 저장한 id값은 key props로 사용한다.
- 마지막으로 ReactDOM으로 랜더링만 사면 끝난다.
```jsx
      ReactDom.render(
           <TodoApp />,
           document.getElementById('todos-example')
      ) 
 ```
 <b>handleSubmit(e)에서 e.preventDefault()메소드를 사용하는 이유</b>
  - 브라우저에서 양식을 제출할 때는 기본적으로 브라우저의 새로 고침이 발생하는데, React나 SPA의 경우 필요가 없는 동작임으로 이를 방지하기 위해 사용된다.

 <b>외부 플러그인을 사용하는 컴포넌트</b>
 - React는 유연하며 다른 라이브러리나 프레임워크를 함께 활용할 수 있다.
 - 이 예제에서는 외부 마크다운 라이브러리인 remarkable을 사용해 textarea의 값을 실시간으로 
변환한다.

<b>Markdown 예제</b>   
 외부컴포넌트를 사용한 markdown에디터 이다.     
 외부 플로그인은 Remarkable을 사용함으로 CDN으로 링크를 추가한다.
```jsx
  class MarkdownEditor extends React.Component {
  constructor(props) {
    super(props);
    this.md = new Remarkable();
    this.handleChange = this.handleChange.bind(this);
    this.state = { value: 'Hello, **world**!' };
  }

  handleChange(e) {
    this.setState({ value: e.target.value });
  }

  getRawMarkup() {
    return { __html: this.md.render(this.state.value) };
  }

  render() {
    return (
      <div className="MarkdownEditor">
        <h3>Input</h3>
        <label htmlFor="markdown-content">
          Enter some markdown
        </label>
        <textarea
          id="markdown-content"
          onChange={this.handleChange}
          defaultValue={this.state.value} />
        <h3>Output</h3>
        <div
          className="content"
          dangerouslySetInnerHTML={this.getRawMarkup()} />
      </div>
    );
  }
}

ReactDOM.render(
  <MarkdownEditor />,
  document.getElementById('markdown-example')
);
```


## [ 11월 10일 ]  
### <b>학습내용</b>
<b>영화 앱 깃허브에 배포하기</b>

     1. package.json 파일을 열어 homepage 키와 키값을 browserslist 키 아래에 추가한다.
      - 깃 허브 계정과 저장소 이름에 주의 [ https://계정.github.io/저장소 이름 ]
     2. package.json 파일에 scripts 키값으로 명령어를 추가한다.
     3. ( git add. / git commit -m "" / git push origin master )의 명령어를 사용하여 깃허브에 업로드
     4. ( npm install gh-pages )의 명령어를 사용하여 gh-pages를 설치한다.
     5. ( git remote -v )의 명령어를 입력하여 업로드한 깃허브 저장소의 주소를 확인한다.
     6. ( npm run deploy )의 명령어를 입력하여 영화 앱을 배포한다.
     7. URL에 'https://계정.github.io/저장소 이름'을 입력해서 확인한다.

<b>리다이렉트 기능 만들어 보기</b>

     1. 주소창에 localhost:3000를 입력하여 이동한 다음 console탭에서 history에 출력된 값을 펼쳐확인한다.
     2. Detail컴포넌트를 교재와 같이 코드를 작성하여 함수형에서 컴포넌트로 변경한 다음 location, history 키를 구조 분해 할당한다.
     3. location.state가 undefined인 경우 history.push("/")를 실행하도록 코드를 수정한다.
     4. 주소창에 /movie-detail으로 입력한 후 결과를 확인한다.
      - Home으로 돌아온것을 확인 할 수있다.
     5. location.state.title을 출력하도록 교재와 같이 코드를 작성한다.
     6. 다시 주소창에 /movie-detail으로 입력한 후 결과를 확인한다.
      - TypeError. Cannot read property 'title' of undefined라는 오류가 발생할 것이다.
      - 오류가 발생한 이유는 Detail컴포넌트는 [render() -> componentDidMount()]의 순서로 함수를 실행하기 때문이다.
      7. location.state가 없으면 render()함수가 null을 반환하도록 if문을 이용하여 수정을 하고 결과를 확인한다.

<b>React의 특징</b>
- 캡슐화된 컴포넌트로 개발되어 재사용이 용이하다.
- 상호작용이 많은 UI개발에 적합하다.
- DOM과는 별개로 상태를 관리할 수 있다.
- 기존 코드와 별개로 개발이 가능하다.
    
<b>CDN: Content Delivery Network 또는 Content Distribution Network</b>   
<b>React App의 개발환경으로 create-react-app을 사용하는 방법 외 CDN방식으로 사용할 수 있다.</b>

     1. 새 html파일을 만든다
     2. minify파일 링크르 사용한다.
     3. crossorigin적용한다.
     4. babel을 CDN으로 적용한다.
     5. script의 type을 text/babel로 설정한다.

<b>state가 포함된 component</b>
- 동적인 데이터는 this.state로 접근할 수 있다.
- state가 변하면 render()메서도가 다시 호출되어 화면이 갱신된다
- 예제는 화면이 켜져있는 동안 초를 카운트하는 timer앱이다
- 초기화 state를 0으로 출력한다.
- 이후 componentDidMount()메소드로 1초에 한번씩 tick()메소드를 호출한다.
- 호출된 tick()메소드는 setState()를 통해 state를 1씩 증가시킨다.      

## [ 11월 03일 ]  
### <b>학습내용</b>
<b>영화 상세 정보 기능 만들어 보기</b>

     1. console.log를 통해 About으로 어떤 props가 넘어오는지 확인한다.
     2. 교재와 같이 Navigation 컴포넌트 /about으로 보내주는 Link 컴포넌트의 to props를 수정한다.
     3. console탭을 통해 state키에 보낸 데이터값을 확인한다.
     4. Navigation컴포넌트를 원래대로 돌려 놓는다.
     5. Movie 컴포넌트에 Link 컴포넌트를 임포트하고, Link 컴포넌트에 to props를 교재와 같이 작성한다.
     6. Detail 컴포넌트를 routes폴더에 추가하고 Detail 컴포넌트에서 Movie 컴포넌트의 Link 컴포넌트가 보내준 영화 데이터를 
     확인할 수 있도록 console.log도 작성한다.
     7. App.js를 연 다음 Detail 컴포넌트를 임포트하고 Route 컴포넌트에 Detail컴포넌트를 교재와같이 작성한다.
     8. 영화 카드를 클릭해서 /movie-detail로 이동한 후 화면에는 Detail 컴포넌트가 출력하고 있는 hello라는 문장을 확인한다. 
     그리고 console탭에서 Movie 컴포넌트에서 Link를 통해 보내준 데이터가 들어있는지 확인한다.
     9. URL에 /movie-detail로 바로 이동한 후 console탭에 영화 데이터가 들어있는지 확인한다. 
     (영화의 데이터가 없어 state가 undefined으로 나온다.)  

<b>내비게이션 만들어 보기</b>

     5. 실수로 Nacigation 컴포넌트를 HashRouter 바깥에 위치시켰는지 확인하고 실수했다면 수정한다.
     6. components폴더에 Navigation,css파일을 만들고 교재와 같이 작성한 다음 Navigation 컴포넌트에 임포트한다.
     또 Navigation.css파일을 Navigation 컴포넌트에 적용시키기 위해 Navigation 컴포넌트의 JSX를 수정한다.

## [ 10월 27일 ]  
### <b>학습내용</b>
<b>내비게이션 만들어 보기</b>

     1. components 폴더에 Navigation.js파일을 생성한 후 교재와 같이 코드를 작성한다.
     2. App 컴포넌트에 Navigation.js를 임포트하고 <HashRouter></HashRouter> 사이에 포함 시킨다.
     3. Home 링크를 눌러서 동작이 잘 되는지 확인한다.
      - 링크를 누를 때마다 리액트가 죽고 새 페이지가 열리는 문제가 발생한다.
     4. Navigation 컴포넌트에 Link 컴포넌트를 임포트한 다음 a 엘리먼트를 Link 컴포넌트로 바꿔서 작성한다.

<b>라우터 만들어 보기</b>

     1. App.js파일에 HashRouter와 Route 컴포넌트를 임포트한 다음 HashRouter 컴포넌트가 Route 컴포넌트를 감싸 반환하도록 수정한다.
     2. About 컴포넌트를 임포트하고 path,component props에 URL과 About 컴포넌트를 전달
     3. About.js를 교재와 같이 코드를 작성한다.
     4. localhost:3000/# 에 path props로 지정했던 값 /about을 붙여서 접속한다.
     5. App 컴포넌트에 Home 컴포넌트를 임포트하고, 또 다른 Route 컴포넌트를 추가한다.
     6. 라우터를 테스트하여 문제가 발생하였는지 찾아본다. (스크롤 맨 아래로 내리면 About 컴포넌트의 내용이 보인다)
     7. 라우터의 동작을 확인하기 위해 교재와같이 App.js파일을 수정한다.
     8. /home , /home/introduction, /about을 URL에 입력하면서 동작들을 확인한다
     9. 동작을 확인했다면 다시 원상태로 코드를 돌려 놓는다. 이전과 같은 문제가 발생한다.
     10. 문제를 해결하기 위해서는 path 앞에 exact={true}를 추가하면 그 문제가 해결된다.
     11. About.css파일을 routes 폴더에 생성하고 About.js와 About.css파일을 교재와 같이 코드를 작성한 후 동작을 확인한다.

<b>Reat-router-dom설치 및 Project File 정리</b>

     1. ' npm install react-router-dom '을 입력하여 패키지 설치
     2. src폴더 안에 components 폴더를 만들고 Movie컴포넌트를 이동
     3. routes폴더를 만들고 Home.js,About.js파일 생성 Home.js에 App.js코드를 복사하여 붙여 넣어야한다.
     4. Home.js에서 코드를 class Home, export default Home로 바꿔야하고 Moive 컴포넌트를 임포트하고, 스타일링을 위한 Home.css도 임포트한다.
     5. routes폴더에 Home.css파일을 만든 후 교재와 같이 코드 입력
     6. App.js를 교재와 같이 수정한 후 브라우저 촉에 맞게 1줄 또는 2줄로 출력되는지 확인한다.

<b>영화 앱 전체 모습 수정하기</b>

     1. App.css파일 내용을 책과 같이 수정하여 글꼴과,배경색 적용
     2. Movie.css파일 내용을 책과 같이 수정 이때 영화 카드는 브라우저 폭에 따라 크기가 조정됨
      - 코드는 github.com/easysIT에서 참고할 수 있음
     3. {summary}를 {summary.slice(0,180)}...로 수정 (시놉시스 180자로 제한하는 방법)
     4. index.html에 <title>태그를 수정하여 영화 앱 제목 바꾸기

<b>영화 앱 전체 모습 수정하기</b>

     7. li tag에 key props 추가하기 
      - genres.map((genre,index)) => <li key={index} className="genres__genre">{genre}</li>와 같이 코드 작성
      - warning이 사라진것을 확인할 수 있다.
       

## [ 10월 20일 ]  
### <b>학습내용</b>
## < <b>중간고사</b> >   

## [ 10월 13일 ]  
### <b>학습내용</b>
<b>Movie 컴포넌트 만들기</b>

     10. Movie 컴포넌트에서 id,title,year,summary,poster props를 받아 출력할 수 있도록 수정
     11. this.state에 있는 movies를 얻은 다음 App 컴포넌트에 출력하는 자리를 movies.map()로 수정
     12. 우선 console탬에 영화 데이터를 출력한 다음, 아무것도 반환하지 않는지를 확인한다.
     13. Movie 컴포넌트를 App.js에 임포트 한 후 movies,map()에 전달한 함수가 <Movie />를 반환하도록 만듬
     14. Moive 컴포넌트에 props를 전달, id,year,title,summary,poster를 isRequired로 설정하였으니, 설정한 props를 모두 전달해야함
     15. console을 확인하면 'Warning:Each child in a list should have a unique "key" prop'라는 오류가 발생할 것이다. 
     16. 그 오류를 수정하기 위해서는 key props를 추가시켜주면 해결이 된다. key={movie.id}를 추가한 후 콘솔을 확인하면 오류가 사라질 것이다.

<b>영화 앱 스타일링하기</b>

     1. App 컴포넌트가 JSX의 바깥쪽을 section class="container"로 Loding...을 <div class="loder">등 교재에 나와있는 데로 App.js의 코드를 수정한다.
     2. Movie컴포넌트에 있는 title,year,summary를 목적에 맞게 태그(HTML)로 감싸자
     3. 전체 태그를 감싸는 div태그를 추가하고 img태그를 div class="movie__data"위에 추가
     4. 완성코드를 확인했을 때 id props가 필요하지 않기에 제거를 해준다.
     5. h3태그에 style 속성을 추가하고 backgroundColor:"red"와 같이 속성값을 입력
     6. src파일에 Movie.css와 App.css파일을 만들자
     7. App,Moive 컴포넌트에 App.css와 Movies.css를 각각 임포트 적용
     8. App.css 파일을 수정, 배경색을 어둡게 변경

<b>영화 앱 전체 모습 수정하기</b>

     1. 작업에 들어가기 전에 App.css의 내용을 제거
     2. runtime 아래 있는 장르 genres를 추가해 준다
     3. App컴포넌트에 Movie컴포넌트에 genres props를 넘겨준다. console을 확인해 보면 두가지의 warning을 확인할 수 있다.
          - Invalid DOM property 'class'. Did you mean 'className'?
          - Failed prop type: The prop 'genres' is marked as required in 'Movie', but its value is 'undefined'.
     4. genres가 undefined인 상태를 너치기 위해서는 App컴포넌트에서 Movie 컴포넌트로 genres props를 전달하면 해결이 된다.
     5. 앞에서 작성한 모든 코드에 있는 JSX의 class 속성 이름을 className으로 바꿔준다. 이와 비슷한 경우로 for 속성을 <label for="name">이 아니라 <label htmlFor="name">와 같이 작성해야 한다.
     6. Movie컴포넌트에서 장르를 출력하도록 코드를 수정하고 genres props가 배열이므로 map()함수를 사용한다. genres props를 ul,li태그로 감싼다. console탭을 확인해보면 warning을 확인할 수 있다.
          - Each child in a list should have a unique "key" prop.

<<<<<<< HEAD

=======
>>>>>>> 21ac11c1aa013a739499ccc04d05eaa78de3b3bf
## [ 10월 06일 ]  
### <b>학습내용</b>
<b>클래스형 컴포넌트의 일생 알아보기</b>

     1. constructor()함수를 선언하고, console.log()함수를 이용해 문장을 출력하기
     2. componentDidMount()함수를 선언하고, console.log()함수를 이용해 문장 출력 및 확인 
     3. componentDidUpadate()함수를 componentDidMount()함수 아래에 선언하고, console.log()를 이용해 문장을 출력 및 확인
     4. componentWillUnmount() 함수를 작성한 한 console.log를 이용해 문자아을 출력하기

<b>영화 앱 만들기 워밍업</b>

     1. App 컴포넌트 비우기
     2. isLoading state를 추가, isLoading state는 컴포넌트가 마운트되면 true여야 하기에 교재와 같이 작성
     3. isLoading state에 따라 '로딩 중이다' , '로딩이 다 됐다'와 같은 문장을 화면에 출력
     4. setTime()함수를 이용하여 5초후 isLoading state를 false로 바꿔보자 교재와 같이 코드 작성
     5. 영화 데이터를 어디에 저장할지 찾기
     6. movies state를 만들기 교재와 같이 배열로 코드 작성

<b>영화 API사용해 보기</b>

     1. axios설치하기 ( npm install axios 입력)
     2. 브라우저에 yts.lt/api라고 입력한 후 yts영화 데이터 api 사이트에 접속 / <List Movies>를 누른 후 'https://yts.mx/api/v2/list_movies.json'주소를 복사
     3. 브라우저에 복사한 주소 입력
     4. chrome web store에서 json viewer 확장 도구 설치
     5. json viewer를 설치한 다음 다시 주소에 접속 후 코드확인
     6. 노마드 코더 영화 api 깃허브'https://github.com/serranoarevalo/yts-proxy'에 접속
     7. '/list_movies.json'주소를 입력하여 접속한 후 코드 확인
     8. '/movie_details.json'를 입력한 후 코드확인
      - 아무것도 출력되지 않는다. 왜냐하면 api가 movie_id라는 조건을 요구하기 때문
     9. yts.mx/api#movie_details에 접속하면 movie_details Endpoint는 movie_id가 필수임을 확인할 수 있다.
     10. App.js파일 맨 위에 axios를 import한 다음, componentDidMount()함수에서 axios로 api를 호출
     11. axios.get()함수의 인자에 URL을 전달하여 API를 호출
     12. devTools에 [Network]탭을 연 다음 영화 앱을 새로 고침하자. 그러면 Name이라는 항목에 list_movies.json이라고 나온 부분이 생긴다.
     13. getMovies()함수를 만들고, 그 함수 안에서 axios.get()이 실행되도록 작성
     14. getMovies()함수에 async와 await 붙이기 교재와 같이 코드 작성
      - 시간이 필요하다느 것을 알리기 위해서는 async, await 키워드가 필요하다.

 - <b>지금까지 했던 내용 간단한 복습</b>
     - 리액트 앱이 실행되면 최초로 render()함수가 실행된다. 최초의 state에는 isLoading,movies가 있다. isLoading는 true이고 movies는 빈 배열일 것이다. 그러다 보니 최초의 실행 화면에는 Loading...이 표시된다. <br>
     App 컴포넌트가 마운트되면 compoenentDidMount()함수가 실행되면서 getMovies()함수가 실행될 것이다. 그런데 getMovies()함수에는 시간이 많이 걸리는 axios.get()이 포함되어 있어서 async와 await를 붙였습니다. 

<b>영화 데이터 화면에 그리기</b>

     1. axios.get()으로 잡은 영화 데이터가 movies 변수 안에 들어 있으니까 console.log(movies)를 통해 출력하여 확인
     2. devTools에 콘솔창 확인
     3. data키를 펼쳐보고, moveis배열까지 펼쳐보기
     4. console.log(movies)에 .data.data.movies를 추가한 후 콘솔창 확인
     5. 교재와 같이 const코드를 작성한 후 console.log에 '.data.data.movies'를 지우기
     6. this.setState({movies:movies})와 같이 작성하고 console.log()는 더이상 사용하지 않기 때문에 제거
     7. {movies:movies}는 키와 대입할 변수의 이름이 같으니까 {movies}로 축약할 수 있기에 this.getState({movies})로 수정
     8. isLoading state를 false에서 true로 업데이트 교재와 같이 코드 작성

 <b>Movie 컴포넌트 만들기</b>

     1. src 폴더에 Movie.js 파일을 새로 만든 후 교재와 같이 코드를 작성
     2. yts-proxy.now.sh/list_movies.json에 접속해서 영화데이터를 확인
     3. 우선 id를 Movie.propTypes에 추가 id는 자료형이 Number이고, 반드시 있어야 하니까 PropTypes.number.isRequired로 작성
     4. year,title,summary,poster를 각각 Movie.propTypes에 추가
     5. yts-proxy.now.sh/list_movies.json에 접속한 다음 키와 키값을 자세히 살펴본다.
     6. yts.It.api#list_movies에 접속한 다음 Encoding Paramerters에 주목 sort_by라는 Parameter를 본다.
     7. Parameter와 Parameter에 넘겨줄 값을 = 으로 이어서 작성하면 되는 거를 확인
     8. .json주소에 접속해 평점을 확인
     9. axios.get()에 json주소 값을 입력

<<<<<<< HEAD

=======
>>>>>>> 08179132a05972771d683005ccc491584971d956
## [ 09월 29일 ]  
### <b>학습내용</b>
<b>리액트와 map()함수가 어떤 상호작용을 하는지 알아본다.</b>
> 실습

     3. 크롬 console을 통해 map()함수의 반환 값을 살펴보자. (console.log(foodILike.map(renderFood)); 명령어 추가)
     4. renderFood()함수는 map()함수가 반환한 리액트 컴포넌트를 출력하려고 사용해 본것이므로 다시 원래대로 코드를 돌려놓자
     5. 리액트의 원소들은 유일해야 하는데 리액트 원소가 리스테에 포함 되면서 유일성이 없어서 오류 메시지가 나온다
     6. 이 문제를 해결하기 위해 foodILike 배열 원소에 id라는 값을 추가
     7. Food 컴포넌트에 key props를 추가하자. (key={dish.id}를 추가)
     8. img 엘리먼트에 alt 속성 추가하기 (alt={name}를 추가)

<b>prop-types도입하기</b>
> 실습

     1. 음식 데이터에 rating 추가하기 (rating: n 을 추가하기)
     2. 터미널에 명령어를 입력해서 prop-types를 설치하기 (npm install prop-types를 입력)
     3. package.json 파일을 열어 dependencies 키에 있는 prop-types값이 들어가 있는지 확인
     4. import PropTypefrom'prop-types'를 App.js 파일 맨 위에 추가 및 교재와 같이 rating props를 Food 컴포넌트에 전달
     5. 교재 이미지와 같이 Food.propTypes를 작성 후 실행
     6. rating에 넘겨준 값이 자료형이 string이였기 때문에 에러 발생
     7. rating:PrpTypes.string.isRequired 대신 rating: PropTypes.number.isRequired로 수정
     8. Food컴포넌트에 전달하는 picture props의 이름을 image로 수정
     9. Food 컴포넌트에 picture props가 아니라 image props를 전달했기 때문에 에러 발생
 - isRequired는 rating: PropTypes.number라고 작성하면 필수가 아니여도 되는 항목이 된다.

<b>state로 숫자 증감 기능을 만들어 보기 1</b>
> 실습

     1. 클래스형 컴포넌트 작성하기 (import Reat from 'react';와 export default App;을 제외한 컴포넌트를 모두 지움)
     2. 교재 이미지와 같이 코드를 작성
      - 클래스형 컴포넌트가 되려면 'App 클래스가 제공하는 Component 클래스를 반드시 상속받아야 한다'
     3. render() 함수를 사용해 교재이미지와 같이 코드를 작성
     4. state를 사용하려면 state={}; 라고 작성하여 state를 정의
     5. state에 count값을 추가하고 render()함수에서 {this.state.count}를 출력하기
     6. <Add> 버튼과 <Minus> 버튼을 추가
     7. add()함수와  minus()함수 작성하기
     8. 버튼을 누르면 동작할 수 있도록 onClick 속성 추가

<b>state로 숫자 증감 기능을 만들어 보기 2</b>
> 실습

     1. this.state.count를 추가
     2. add()함수와 minus()함수가 동작하는지 확인
     3. 리액트는 state가 변경되면 render()함수를 다시 실행하여 변경된 state를 화명에 출력하는데 state를 직접 변경하는 경우에는 render()함수를 다시 실행하지 않아 에러가 발생한다.
     4. setState()함수를 사용해서 state의 값을 변경
     5. 교재 이미지와 같이 수정하여 count state의 값을 증가 또는 감소 시키기
     6. add,minus()함수를 교재 이미지와 같이 개선

## [ 09월 15일 ]  
### <b>학습내용</b> 

<b>Props</b>   
- Props란 컴포넌트에서 컴포넌트로 전달하는 데이터를 말한다.   
- 함수의 매개변수 역활을 하는 것이다.   
- 따라서 props를 사용하면 컴포넌트를 효율적으로 사용할 수 있다.

> 실습

     1. 앞서 수정한 Potato컴포넌트의 이름을 Movie로 바꿔보자.   
     2. 당연한 결과지만 출력 화면을 보면 리스트의 내용이 모두 동일 하다. 영화 앱의 리스트라면 아마도 리스트의 값이 모두 달라야 할 것이다.   
     3. 컴포넌트의 이름을 Movie에서 Food로 변경하고 Movie컴포넌트는 모두 삭제한다.   
     4. props에는 불리언 값,숫자,배열과 같은 다양한 형태의 데이터를 사용할 수 있다. props의 전달 데이터는 문자열인 경우를 제외하면 모두 중과호로 감싸야 한다.   
     5. 다양한 값들을 props를 통해 전달하기 위해 코드를 수정해 본다. 문자열을 제외한 모든 값들은 중괄호로 묶는 것을 잊지 말자.   
     6. 아무런 변화가 없을 것이다. 이는 Food컴포넌트에 props를 전달 했을 뿐 아직 사용하지 않았기 때문이다.   
     7. Food컴포넌트의 인자로 전달된 props를 출력 후 console을 확인한다.   
     8. Food컴포넌트에 전달한 props를 소성으로 하는 객체(object)가 출력된 것을 확인할 수 있다.   
     9. 코드에서 props전달 값 중에서 kimchi만 놔두고 모두 삭제한 후 전달 값을 다시 확인해 본다.   
     10. 객체의 특정 값을 사용할 대는 점(.) 연산자를 사용한다.   

<b>구조 분해 할당으로 props사용</b>   
- ES6의 문법 중 구조 분해 할당를 사용하면 점 연산자를 사용하지 않아도 된다.   
- 데이터의 개수가 많아지면 구조 분할 할당을 사용하는 것이 편리하다.   

<b>비슷한 컴포넌트 여러개 만들기</b> 
> 실습

     1. App.js파일을 다시 열어 코드가 효율적인지 살펴본다.   
     2. 서버에서 넘어온 데이터를 저장할 수 있도록 foodILike라는 변수를 만든 다음 빈 배열을 할당한다. 비효율적으로 작성된 Food 컴포넌트는 모두 삭제한다.   
     3. 서버에서 데이터가 넘어온다고 가정하고 [ 교재 이미지 참고 ]과 같이 코드를 작성해 본다. 데이터 부분은 반복되기 때문에 필요한 만큼 작성한다.   

<b>map함수로 컴포넌트 많이 만들기</b>
> 실습

     1. friends라는 배열을 선언하고 친구 이름을 입력한다. (만일 node가 설치되어 있다면 node를 사용해도 된다.)   
     2. 배열의 이름을 입력하면 데이터를 확인할 수 있다.   
     3. map()함수는 배열의 모든 원소 마다 특정 작업을 하는 함수를 적용하고, 그 함수가 반환한 결과를 ㅁ모아서 배열로 변환해 준다. 배열 데이터를 하나씩 출력한 후, 리턴 값을 모아서 만든 배열[0,0,0,0]을 반환한다.
     4. 앞서 current라는 인자는 반드시 사용해야 하는 것은 아니다. 원하는 이름을 상용하면 된다. 여기서는 friend를 사용한다. 중요한 것은 인자의 이름이 아니라 배열의 원소가 1개씩 전달 되면서 함수가 반복 실행 된다는 것이다.   
     5. 앞에서 와는 다르게 화살표 함수가 아니라 무명함수를 통해 배열의 원소를 하나씩 전달 받는다. 전달받은 원소에 하트 케릭터를 추가하여 리턴한다.   
     6. foodILike 배열을 살펴 보면서 map()함수를 어떻게 이용할 지 생각해 보자. foodILike.map()의 형태로 지정하고, 인자는 dish => <Food ... />와 같이 컴포넌트를 반환하는 함수를 전달한다.   
     7. dish에 foodILike 배열에 있는 원소가 하나씩 넘어가고, Food 컴포넌트에 dish.name 처럼 음식이름을 dish prorps로 넘겨준다. 앞으로 개발할 영화 앱에서도 같은 구조로 사용하게 된다.
     8. picture props에는 dish.picture가 추가 된다.
     9. 이미지 출력을 위해 Food에 <img> tag를 추가한다.


<b>리액트와 map()함수가 어떤 상호작용을 하는지 알아본다.</b>
> 실습

     1. 다음과 같이 App.js파일을 수정한다. [ 교재 이미지 참고 ]   
     2. 함수의 return값은 앞서했던 (9.)해당 부분과 동일하다.

## [ 09월 08일 ]  
### <b>학습내용</b>

1. 터미널 Powershell에 아래와 같이 입력하여 폴더 생성
     > npx create-react-app movie_app_2021-4   

2. movie_app_2021-4 폴더를 생성한 movie_app_2021-4로 파일을 연 다음 npm start를 입력하여 재대로 설치가 되었는지 확인
     - 실행 종료시 ctrl + c 입력 

3. 실행이 잘 되는 것을 확인한 후 자신의 Githup에 올림

4. movie_app_2021-4 폴더에서 아래에 적혀있는 불필요한 파일 제거
     > App.css   
     App.test.js   
     index.css   
     logo.svg   
     reportWebVital.js   
     setupTest.js   
     package-lock.js

5. index.js파일에서 필요없는 문장들 삭제
     > import './index.css';   
    import reportWebVitals from './reportWebVitals';   
    <React.StrictMode>   
    </React.StrictMode>   
    reportWebVitals();
    - 수정 시 ' , '를 지우지 않도록 주의

6. App.js 파일 수정

7. index.js와 index.html에 있는 id="root"를 potato로 수정한 후 정상적으로 작동하는 것을 확인
    - 위 과정은 리액트 동작 원리를 알아보기 위한 연습 과정이다. 
    - index.js에서 app.js의 리턴값을 가져다가 index.html에 전달해준다.
8. 정상작동이 확인 될 시 potato를 다시 root로 수정

컴포넌트
- function으로 정의 내린 곳을 컴포넌트라고 한다.
- Index.js파일로 컴포넌트의 사용 알아보기
    - index.js 파일을 열어서, < App />이라고 입력한 부분을 잘 살펴본다.   
< App >을 ReactDOM.render()함수의 첫 번째 인자로 전달하면 App 컴포넌트가 반환하는 것들을 화면에 그릴수 있다.   
App 컴포넌트가 그려질 위치는 ReatDOM.render()함수의 두 번째 인자로 전달하면된다.   
컴포넌트를 사용할 때 <App/>이 아니라 App라고만 입력하면 오류가 발생한다.   
리액트는 컴포넌트와 함께 동작하고, 리액트 앱은 모두 컴포넌트로 구성된다.

9. src폴더에 Potato.js파일을 생성한 후 function 입력

10. index.js파일 potato를 import선언한 후 < App /> 옆에 potato를 입력 후 오류가 뜨는지 확인
     -오류가 뜨는 이유는 리액트는 최종적으로 단 한개의 컴포넌트를 그려야하는데, 현재 두개의 컴포넌트를 그리려 해서 오류가 발생한 것   
문제를 해결하기 위해서는 photato컴포넌트는 app 컴포넌트 안에 넣어줘야 한다

11. index.js파일에 입력한 potato를 지우고 App.js에 import를 넣어주고 div안에 potato를 입력한 후 정상적인 작동이 되는지 확인

12. Potato파일에 function부분을 복사한 후 App.js파일에 붙여넣기 한 후 potato파일을 삭제

## [ 09월 01일 ]  
### <b>학습내용</b>   
........
