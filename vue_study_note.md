# vue.js & todo-list

## 1. vue.js란? 
* MVVM 패턴에서 ViewModel 레이어에 해당하는 화면단 라이브러리이자 프레임워크.
* 실시간 데이터 바인딩을 제공하고, 화면의 구성 요소들을 컴포넌트 형태로 제공해준다.
* React, Angular에 이어서 모던 프론트 개발에서 가장 많이 사용되는 프레임워크다.

## 2. 데이터 바인딩: vue 객체의 데이터와 HTML node의 연결

[app.js]
```javascript
  var app1 = new Vue({ // vue 인스턴스 기본
                el: '#app',
                data: {
                    vueMessage: '이 텍스트는 vue 객체의 속성 값으로 dom객체와 바인딩 될 예정!'
                }
	});
```

[index.html]
```html
<div id="app">
	{{ vueMessage }} 
</div>

```
`{{ }}` 중괄호 두개(머스태시) 사이에 변수명을 집어넣으면 vue가 해당 변수를 찾아 출력해준다.  
여기서는 `#app` 와 바인딩된 vue 인스턴스의 `vueMessage` 변수의 값이 리턴된다.  
이것이 Vue의 가장 기본적인 데이터 바인딩 방법이다.


## 3. input 태그와 v-model: v-model 프로퍼티를 이용해 사용자 입력값 가져오기
프론트 개발을 하다보면 폼 태그를 이용해 사용자가 입력한 데이터 값을 가져와 저장하고,  
이것을 다시 뷰단에 활용하는 작업을 많이 하게 된다.
일반적인 javascript 개발에서는 이 작업을 다음과 같은 코드로 수행한다.

### A. javascript 방식
  
[index.html]
```html
<div id="app_form">
	<input type="text" id="user_id" onchange="userIdChanged(this)">
	<input type="password" id="user_password">
	<button type="button" onclick="login()">로그인</button>
	<span class="user_id_text"></span>
</div>

```

[app.js]
```javascript
 function login(){ //입력값을 가져오는 메소드 
	let userId = document.querySelector("#user_id").value;
	let userPassword = document.querySelector("#user_password").value;
	console.log(userId, userPassword);		
};

function userIdChanged(obj){//가져온 값을 다시 HTML node에 반영해주는 메소드
	document.querySelector(".user_id_text").innerText  = obj.value;
}
```

`.value` 메소드로 값을 가져오고, `.innerText` 메소드로 해당 값을 반영해주는 방식.  
이 경우 사용자 입력값에 변화가 있을 때 마다 두 함수를 실행해줘야 실시간으로 값이 반영된다.


### B. Vue 방식
[index.html]
```html
<div id="app_form">
	<input type="text" id="user_id" v-model="userId">
	<input type="password" id="user_password" v-model="userPassword">
	<button type="button" >로그인</button>

	<div class="check-vue-print">
		<p><span class="id">당신의 아이디: </span> <b> {{ userId }} </b></p>
		<p><span class="pw">당신의 pw:</span> <b> {{ userPassword }} </b> </p>
	</div>
</div>
```

[app.js]
```javascript
var app_form = new Vue({ 
	el: '#app_form',
	data(){ //ES6
		return {
			userId:'',
			userPassword:''
		}	
	}
});
```

`v-model`을 통한 form 데이터 값을 실시간으로 가져오는 방식이다.  
굳이 함수를 실행해주지 않아도 vue가 실시간으로 변화를 감지하고 반영해준다.  
구체적으로 `v-model` 속성 값으로 변수명을 지정해주면,  
자동으로 input 태그의 value 값과 vue 인스턴스의 변수값이 동기화된다.  
이를 통해 손 쉽게 사용자가 입력한 값을 가져와 실시간으로 출력하는 것이 가능해진다.


## 4. 데이터로 목록 출력하기
개발을 하다보면 데이터를 바탕으로, 목록을 출력하는 일이 잦다. 
기존 javascript에서는 `for문`, `for-in문`, `forEach` 등을 활용해서 이 작업을 수행했다.
동일한 작업을 vue의 `v-for` 프로퍼티를 통해 수행할 수 있다.
  
[index.html]  
```html
<div id="app_list">
	<ul>			
		<template v-for="item in items">
			<li v-for="item in items">
				<img :src="item.image" alt="">
			</li>
		</template>
	</ul>
</div>
```

[app.js]  
```javascript
var app_list = new Vue({ 
	el: '#app_list',
	data(){ 
		return {
			items: [ // json 객체로 이뤄진 data 배열 
				{
					id:1,
					image: 'https://picsum.photos/210/118/?image=1'
				},
				{
					id:2,
					image: 'https://picsum.photos/210/118/?image=2'
				
				},
				{
					id:3,
					image: 'https://picsum.photos/210/118/?image=3'
				
				},
				{
					id:4,
					image: 'https://picsum.photos/210/118/?image=4'
				
				}
			]
		}	
	}
});
```
  
`v-for` 속성을 지정해주고 속성값으로 for-in문을 지정.  
items 변수(배열)값의 각 배열 요소들을 하나씩 가지고와 실행한다.(item = 각 배열의 요소)
  
## 5. 태그 속성 바인딩
vue.js를 이용해 HTML 태그의 `src`나 `class`와 같은 속성(attribute)의 값을 유동적으로 변경해 줄 수 있다.  

### (1) v-bind:
  
[index.html]  
```html
<div id="app_attr">
	<img v-bind:src="img" alt="">
	<img :src="img" alt="">
</div>
```
속성 앞에 `:`를 붙이고 값에 변수명을 넣어주면,  
HTML 태그와 바인딩 된 vue 객체의 변수명과 속성 값이 연결된다.
본래는 `v-ind:src`가 정식 표현이지만 축약표현인 `:src`만 써도 적용이 되는 것.  
  
[app.js]
```javascript
 var app_attr = new Vue({ 
	el: '#app_attr',
	data() {
		return {
			img: "https://picsum.photos/200/200/?image=353",
			id: 1
		}
	}
 });
```
  
### (2) v-bind:id
   
마찬가지로 이를 이용해 id나 class 값을 변수와 연동하여 유동적으로 적용시켜줄 수 있다.

[index.html]  
```html
<div id="app_attr">
	<img :src="img" :id="`thumb_${id}`" alt="">
</div>
```

ES6의 백킷을 활용해 변수명을 적어준다.  
정상적으로 id 값으로 `thumb_1` 출력됨을 확인할 수 있다.  

### (3) v-bind:style
  
css 스타일도 변수로 넘겨서 지정이 가능하다.  
[index.html]  
```html
<div class="style-binding" :style="style">
	<p>style binding with vue.js</p>
</div>
```
  
[app.js]  
```javascript
 var app_attr = new Vue({ 
	el: '#app_attr',
	data() {
		return {
			img: "https://picsum.photos/200/200/?image=353",
			id: 1,
			style: {
				background: "#111",
				fontSize: "18px",
				color: "#fff",
				textAlign: "center"
			}
		}
	}
 });
```

혹은 vue 객체 내에서 스타일을 json 형태로 여러개의 변수로 분리해도,  
배열 형태로 변수 여러개를 넘겨줄 수 도 있다. 

[app.js]  
```javascript
 var app_attr = new Vue({ 
	el: '#app_attr',
	data() {
		return {
			img: "https://picsum.photos/200/200/?image=353",
			id: 1,
			divStyle: {
				background: "#111",
			},
			fontStyle: {
				fontSize: "18px",
				color: "#fff",
				textAlign: "center"
			}
		}
	}
 });
```
  
[index.html]  
```html
<div class="style-binding" :style="[divStyle, fontStyle]">
	<p>style binding with vue.js</p>
</div>
```

### (4) v-bind:class 를 이용해 toggle class 관리하기
  
v-bind를 이용해 class를 추가, 삭제하는 것도 유동적으로 가능핟.

[index.html]  
```html
<div class="class-binding" :class="[ownClass, toggleClass]">
	<p>class value binding with vue.js</p>
</div>
```

[app.js]  
```javascript
 var app_attr = new Vue({ 
	el: '#app_attr',
	data() {
		return {
			ownClass: 'ownClass',
			toggleClass: {
				on: false
			}
		}
	}
 });
```
  
바인딩된 html 태그에 `ownClass` 클래스는 추가되었지만,  
토글 클래스인 `on`은 값이 `false`로 설정되었기 때문에 클래스가 추가되지 않았다.  
이를 `true`값으로 바꿔주면 자동으로 클래스가 추가 되는 것을 확인할 수 있다.  
브라우저 콘솔에서 한번 vue 객체 변수 값을 바꿔보자!

> app_attr.toggleClass.on = true;
  
## 6. vue 인스턴스 내 메소드 선언
vue에서는 외부에서 사용할 함수들을 methods 라는 json 객체를 만들어 그 안에서 선언해 관리해준다.  
주의할 점은 선언하는 함수는 화살표 함수를 사용하면 안된다는 점!
  
HTMl 태그에 `v-on:` 프로퍼티를 설정해줌으로서 vue 인스턴스(객체)와의 메소드 바인딩이 가능하다.
  
[index.html]  
```html
<div id="app_method">
	<button type="button" :class="[buttonClass]" v-on:click="print">print 메소드 실행</button>
</div>
```

[app.js]  
```javascript
var app_method = new Vue({ 
	el: '#app_method',
	data() {
		return {
			counter: 0,
			buttonClass: "buttonClass",
		};
	},
	methods:{ 
		print(){
			console.log(this.counter)
		}
	}
});
```
  
버튼을 클릭할 때 마다 `add()` 메소드가 실행되고 콘솔에 0이 찍힌다.
  
혹은 `v-on:이벤트명` 의 값에 직접적으로 javascript 코드를 넣어서 연산도 가능하다.

[index.html]  
```html
<div id="app_method">
	<div class="method-direct">
		<button type="button" :class="[buttonClass]" v-on:click=" counter ++; "> counter 변수 값 더하기 </button>
		<p>COUNTER VALUE: <span>{{ counter }}</span></p>
	</div>
</div>
```
  
v-on:click 프로퍼티의 값으로 javascript 코드 `counter ++` 를 넣어줬다.    
이제 click 이벤트가 발생할 때 마다 `counter ++` 연산작업이 실행된다.  
하지만 이렇게 직접적으로 코드를 쓰는 것은 가독성이 떨어지기 때문에 보통은  
메소드 명을 적어주고, vue 인스턴스에 메소드를 선언해준다.
  
[index.html]  
```html
<div id="app_method">
	<div class="method-direct">
		<button type="button" :class="[buttonClass]" v-on:click="add"> counter 변수 값 더하기 </button>
		<p>COUNTER VALUE: <span>{{ counter }}</span></p>
	</div>
</div>
```

[app.js]  
```javascript
var app_method = new Vue({ 
	el: '#app_method',
	data() {
		return {
			counter: 0,
			buttonClass: "buttonClass",
		};
	},
	methods:{ 
		add(){
			this.counter ++;
		}
	}
});
```
  
`v-on:`도 `v-bind:`의 축약형이 `:`이었던 것 처럼 축약 표현도 있다.
`@click` 식으로 `@`로 축약해서 표현이 가능하다.

[index.html]  
```html
<div id="app_method">
	<div class="method-direct">
		<button type="button" :class="[buttonClass]" @click="add"> counter 변수 값 더하기 </button>
		<p>COUNTER VALUE: <span>{{ counter }}</span></p>
	</div>
</div>
```
  
## 7. vue 메소드로 특정키 활용과 이벤트 버블링 막기
vue 메소드를 활용하면 특정키가 눌렸을 때만 메소드를 실행하게 한다거나,  
이벤트 버블링을 쉽게 막을 수 있다. 
  
만약 엔터키가 눌렸을 때 특정 코드를 작동시키고 싶다고 한다면,
일반적으로 다음과 같은 코드를 작성한다.
  
[index.html]  
```html
<div id="app_keyup">
	<input type="text" @keyup="keyUp">
</div>
```
  
[app.js]  
```javascript
var app_keyup = new Vue({ 
	el: '#app_keyup',
	data() {
		return {
		};
	},
	methods:{ 
		keyUp(e){
			if(e.keyCode !== 13){
				console.log("엔터키를 눌러주세요");
				return;
			}
			console.log("E N T E R");
		}

	}
});
```
enter 키가 눌렸을 때만 콘솔에 `E N T E R`가 찍힌다.  

그런데 이 복잡한 코드를 vue에서 더욱 간단하게 쓸 수 있다.
`@keyup.enter`

[index.html]  
```html
<div id="app_keyup">
	<input type="text" @keyup.enter="keyUp">
</div>
```
   
## 8. vue.js 컴포넌트
vue.js 컴포넌트는 여러가지 태그를 모아 일종의 사용자 정의(custom) 태그를 만드는 기능이다.  
반복적으로 사용되는 HTMl 노드와 태그 뭉치를 일일히 하드코딩 해주지 않고,  
컴포넌트로 모듈화시켜 원할 때 마다 쉽게 가져와 사용하는 방식이라 할 수 있다.  
  
vue 컴퍼넌트 인스턴스를 생성해주는 방식으로 사용을 시작한다.
  
[app.js]  
```javascript

Vue.component("product", { 
	props: ['image', 'title', 'desc'],
	template: `
		<div class="card">
			<div class="card-wrap">
				<div class="thumb">
					<img :src="image" alt="">
				</div>
				<p class="title">{{ title }}</p>
				<p class="desc">{{ desc }}</p>
			</div>
		</div>	
	`
}); 
```

* 첫번째 인자로 태그명을, 두번째 인자로 설정값을 json 형식으로 넘겨준다.  
* 외부에서 속성값으로 입력받을 항목의 값들을 `props` 안에 배열의 형태로 적어준다.  
* HTML 태그 뭉치는 template의 값으로 템플릿스트링(백킷) 기능을 활용해 적어준다.
* 템플릿 스트링 안에서 외부에서 받는 값들이 반영되도록 vue.js 문법을 활용해 변수들을 연결해준다.

  
## 9. vue.js 를 이용해 Todo list 만들기(vue cli 활용하기)

### (1) 개발환경 구축하기: vue cli
todo list 프로젝트에 앞서 vue cli를 활용하기 위해 터널을 실행하고,  
디렉토리를 이동해 vue cli를 설치해준다.
> npm install @vue/cli
  
문제 없이 설치가 되었다면 버전을 확인해본다.
> vue --version
  
이제 원하는 작업 디렉토리에서 vue todo 프로젝트를 설치해본다.
> vue create todo

몇가지 설정값을 확인해주면 vue cli 에서 지원하는 npm 환경의 todo 프로젝트가 설치된다.  
설치된 파일 디렉토리, 구조를 살펴보자.
  
* node_modules: 개발에 필요한 패키지들
* public: 파비콘, index.html(vue.js 아웃팅 결과를 담아줄 HTML node가 위치해있다.)
* src: assets(이미지등 데이터, 자료), components(익스포팅 해 올 Vue components), main.js(webpack이 파일을 읽어들이는 시작지점)
  
`package.json`을 확인해보면 설치된 패키지들과 npm script 를 확인할 수 있다.
  
* serve: 작업을 실시간으로 확인할 수 있도록 로컬 서버 구동
* build: 개발 완료 후 서버에 올리기 전에 필요한 파일들을 빌드업
* lint: 문법을 확인 > 작업을 방해한다. 어떻게든 disable 시켜보자...
  
이제 로컬 서버를 구동해본다.(작업 디렉토리로 이동해서 실행 할 것)
> npm run serve
  
이제 8080포트에서 vue 서버가 대기중이다.
   
### (2) 부트스트랩 CDN 연결

원활한 개발을 위해 css preset 을 제공해주는 부트스트랩 소스를 연결해준다.
```html
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css" integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj7Sk" crossorigin="anonymous">
```
   
기존 vue 프로젝트에 담겨있던 쓸모 없는 컴포넌트, 코드들은 지워준다.  

### (3) vue 코드 작성
강의를 따라 차례차례 코드를 작성해준다.  
로컬 서버에서 실시간으로 변경된 화면을 확인해준다.

```vue
<template>
    <div id="app">
        <div class="container">
	    <div class="col-md-6 offset-md-3">
    		<h1 class="text-center mb-4">TODO App</h1>
    		<input type="text" class="form-control" v-model="userInput" @keyup.enter="addNewTodo()">

                <div class="list-group mt-4"> <!--사용자가 입력한 todolist 값을 출력한 div 태그 -->
                    <template v-for="todo in activeTodoList" > <!-- todolist 배열을 활용해 list 출력. todolist는 현재 상태 설정과 일치하는 배열만 출력해주기 위해 filter 함수를 적용시켜준다.-->
                        <!--
                            <Todo :label="todo.label"
                                @componentClick="toggleTodoStae(todo)"; //모듈화 한 경우
                                //componentClick은 모듈 내부에서 발생시킨 메소드다.
                        />-->
                        <button class="list-group-item text-left" @click="toggleTodoStae(todo)">
                            {{ todo.label }} <!-- todo 객체의 label 변수 값만 출력해준다. 모듈화 하지 않은 경우 -->
                        </button>
                    </template>
                </div>

                <div class="text-right mt-4">
                    <button type="button" class="btn btn-sm" @click="changeCurrentState('active')">할 일</button>
                    <button type="button" class="btn btn-sm" @click="changeCurrentState('done')">완료</button>
                    <button type="button" class="btn btn-sm" @click="changeCurrentState('all')">전체</button>
                </div>
    	    </div>
        </div>
    </div>
</template>

<script>

import Todo from './components/Todo';
export default {
	name: 'app',
	data(){
		return {
			userInput:'',
			todoList:[],
			currentState: 'active'
		};
	},
	computed: { 
		activeTodoList(){
		    return this.todoList.filter( todo => this.currentState==='all' || todo.state === this.currentState );
		}
	},
	methods:{
		changeCurrentState(state){
		    this.currentState = state;
		},
		addNewTodo(){
		    if(this.userInput !== ""){
			this.todoList.push({
			    label: this.userInput,
			    state: 'active'
			});
			this.userInput = ''; 
		    }
		},
		toggleTodoStae(todo){ 
		    todo.state = todo.state === 'active'? 'done' : 'active' ;
		},
		checkActive(todo){ 
		  if( todo.state === 'active') return true;
		}
	},
	components: {
		Todo 
	}
}


</script>
```

### (4) todolist vue 인스턴스 구조 살펴보기

#### vue 컴포넌트(components)
```javascript
import Todo from './components/Todo'; 

components: {
	Todo 
}
```
* import Todo: 목록으로 출력할 todo 태그들을 외부 컴포넌트로 분리 및 모듈화 시켜놓았다. 이를 임포팅 시켜준다.
* 임포팅 해온 `Todo` 객체를 vue 인스턴스 안에서 컴포넌트로 사용하겠다고 선언해준다.

#### vue 데이터(data)
```javascript
data(){
	return {
		userInput:'',
		todoList:[],
		currentState: 'active'
	};
}
```
* userInput: 사용자의 입력 값을 실시간으로 받는 `userInput` 변수. `v-model` 프로퍼티를 통해 폼 태그와 바인딩 되어있다.
* todoList: 사용자가 입력한 값을 받아와 담아두는 배열. 실제 목록을 출력할 때 참조하는 데이터 목록. 
* currentState: 목록의 상태를 알려주는 상태 변수 값. `active`가 default.

#### vue 컴퓨티드(Computed)
```javascript
computed: { 
	activeTodoList(){ 
	    return this.todoList.filter( todo => this.currentState==='all' || todo.state === this.currentState );
	}

```
* activeTodoList(): 목록의 상태를 알려주는 변수 `currentState`에 따라서 알맞게 데이터 목록을 반환 해주는 필터 함수. 만약 `currentState`가 `all` 이면 논리연산문이 무조건 참이므로 전체 목록을 반환해주고, `all`이 아닌 경우에는 `currentState`와 일치하는 상태 값을 가진 배열 요소만 반환한다. 
  
> computed 와 method
> activeTodoList()는 methods 탭이 아닌 computed 탭에 속해있는데, 이는 activeTodoList를 값을 가져오는 getter 함수로 사용하기 위함이다.
> 기능적으로 activeTodoList() 동일하게 함수로서 작동하나 computed 탭에 속할 경우 아래의 특징을 갖는다.
> 1. template에서 호출시 ()를 적지 않는다.  
> 2. return 값이 반드시 존재해야한다.  
> 3. 파라미터를 받지 못한다.  
> 일반 메소드의 하위호환 같아 보이는 computed의 함수. 
> computed 함수는 vue가 비생산적인(쓸데없는) 계산을 하지 않게 해 메모리저하를 막아주는 장점을 갖는다.
> methods에 정의된 함수들은 자신이 참조하고 있는 데이터 값들이 변하지 않더라도,
> 화면에 무언가 변화가 일어나면 무조건 실행이 되는 반면 
> computed 함수들은 자신이 참조한 데이터 값이 변할 때만 실행된다. 
   
참고   
* [계산된 속성 computed, computed vs methods / VueJS Instance / 맨땅에 VueJS](https://medium.com/@hozacho/%EB%A7%A8%EB%95%85%EC%97%90-vuejs-%EA%B3%84%EC%82%B0%EB%90%9C-%EC%86%8D%EC%84%B1-vuejs-instance-computed-93cb6ad7dca9)

#### vue 메소드(Methods)
* changeCurrentState(state): 넘겨 받은 인자로 목록의 상태 변수인 `currentState`의 값을 바꿔준다. default 는 `active`.
* addNewTodo(): 사용자 입력한 값을 `todoList` 배열에 json 요소로 추가해주는 메소드. 배열에 요소를 추가한 후에는 사용자 입력 변수를 초기화 시켜준다.
* toggleTodoStae(todo): 인자로 받은 todo 객체의 `state` 속성값을 토글링(active⇔done) 해주는 메소드
* checkActive(todo): 인자로 받은 todo객체의 `state` 속성값이 `active` 인지 아닌지 판별해주는 메소드. `active`면 `true` 반환.
