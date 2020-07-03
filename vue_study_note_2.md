# vue.js & 맨땅에 VueJS

* [맨땅에 VueJS](https://medium.com/@hozacho/)

## 1. vue 디렉티브 vind-ocne, v-once
html 태그의 속성값에 접근하는 vue 디렉티브 v-bind는 이미 알아봤다.  
v-once는 조금 쓰임이 다른데, 아래와 같은 특수한 경우에 사용된다.

### 일반적인 바인딩
```html
<div id="app_once">
	<h1>{{ title }}</h1>
	<h2>{{ title }}</h2>
	<p>{{ afterBinding() }}</p>
</div>
```
   
```javscript
var app_once = new Vue({ 
	el: '#app_once',
	data: {
		title: 'current title'
	},
	methods: {
		afterBinding(){
			this.title = 'text will be chage into this message if methods are being active!';
			return this.title;
		}
	}

});
```
   
h1 태그와 h2 태그는 각각 vue 인스턴스의 `title` 변수와 바인딩 되어 있다.  
브라우저 렌더링이 시작되면 h1, h2 태그는 각각 `title` 변수의 최초 설정 값 'current title' 으로 출력이 되었다가,  
`afterBinding()`이 실행됨에 따라 새로 지정 받은 값 'text will be chage .... being active!'으로 다시 출력된다.  

### 한번만 바인딩을 원한다면?
그런데 만약 여기서 h1 태그의 innerHTML 값을 최초에 출력받은 'current title'으로 유지하고 싶다면?
  
이런 경우를 위해 등장한 프로퍼티가 바로 `v-once` 프로퍼티다.  

```html
<div id="app_once">
	<h1 v-once>{{ title }}</h1>
	<h2>{{ title }}</h2>
	<p>{{ afterBinding() }}</p>
</div>
```

v-once 프로퍼티가 입력되면, 바인딩 된 태그는 HTML코드로 출력이 된 이후에  
어떤 후처리가 있더라도 *최초에 출력한 값*을 유지시켜준다.  

### 실행결과
h1 태그에는 최초 설정 값인 'current title'이,  
h2 태그에는 최종 메소드 실행 결과 출력 값인 'text will be chage .... being active!'이 출력된다.

   
## 2. vue 디렉티브 v-html
v-html은 바인딩 된 태그 내부에 직접적으로 html코드를 삽입해주는 디렉티브다.

### v-html 을 쓰지 않을 경우
HTML 코드가 data 변수로 선언되어 있을 때,  
해당 값을 그대로 html 어딘가에 출력해주고 싶다면 어떻게 해야할까?  
한번 머스태시를 사용해 변수를 출력해보자.  

```html
<div id="app_html">
	<p>{{ aTag }}</p>
</div>
```

```javscript
var app_html = new Vue({ 
	el: '#app_html',
	data: {
		aTag: '<a href="www.naver.com">NAVER</a>'
	}
});
```

결과는? 변수에 담겨 있던 HTMl 태그가 돔 요소가 아닌 *문자열로 그대로* 출력되어 버린다.  
이때를 위해 사용하는 디렉티브가 바로 `v-html`!

### v-html 을 사용해보자
```html
<div id="app_html">
	<p v-html="aTag"></p>
</div>
```
결과: 정상적으로 p 태그 안에 a 태그가 삽입되었다!


## 3. vue 이벤트리스너 v-on: or @
`v-on:` 디렉티브를 사용하여 돔에 *이벤트리스너*를 추가할 수 있다.  
`v-on:` 뒤에 이벤트 트리거의 종류를 적어주면 된다. 이때 `v-on:`은 `@`로 축약해서 쓸 수 있다.   

```html
<div id="app_event">
	<button v-on:click="onClick">클릭</button>
	<button v-on:dblclick="onDoubleClick">더블클릭</button>
	<button v-on:mousedown="onMouseDown" v-on:mouseup="onMouseUp">꾸우우욱</button>
	<input v-on:keyup.enter="pressEnter" v-on:keyup.delete="pressDelete" v-on:keyup.up="pressUp" v-on:keyup.down="pressDown" v-on:keyup.space="pressSpace" placeholder="텍스트를 입력하고 키보드를 조작해보세요">
</div>
```

```javscript
var app_event = new Vue({ 
                el: '#app_event',
                data: {
					
                },
		methods: {
			onClick(){
				console.log("you just click the button");
			},
			onDoubleClick(){
				console.log("you just 'DOUBLE' click the button");
			},
			onMouseDown(){
				console.log("you just press down the button");
			},
			onMouseUp(){
				console.log("you just press up the button");
			},
			pressEnter(){
				console.log("ENTER");
			},
			pressSpace(){
				console.log("Space");
			},
			pressUp(){
				console.log("up!");
			},
			pressDown(){
				console.log("down!");
			},
			pressDelete(){
				console.log("delete");
			}
		}
});

```


## 4. 조건부 렌더링 v-if, v-else

### 원하는 태그만 렌더링 하고 싶다면..?
`v-if`, `v-else`는 변수의 값 등 어떠한 조건에 따라,  
태그 요소를 선택적으로 브라우저에 렌더링(출력)해주도록 돕는 디렉티브다.

```html
<div id="app_if">
	<h3>당신의 지금 상태는?</h3>
	<p>매우 좋아 :)</p>
	<p>좋지 않아 :(</p>
</div>
```
나의 상태를 나타내는 값이 어딘가의 변수로 존재한다고 가정하고,  
그 변수의 값에 따라 화면에 다음의 두개의 p태그 중 하나만 출력하고 싶다면 어떡해야 할까?  
이때 사용하는 것이 `v-if`, `v-else` 이다.

```html
<div id="app_if">
	<h3>당신의 지금 상태는?</h3>
	<p v-if="mystatus =='good'">매우 좋아 :)</p>
	<p v-else>좋지 않아 :(</p>
</div>
```

```javascript
var app_if = new Vue({ 
	el: '#app_if',
	data: {
		myStatus: 'good'
	},
	methods: {
		changeStatus(){
			this.myStatus == 'good' ? this.myStatus = 'bad' : this.myStatus = 'good';
		}
	}
});
```

`v-if`는 값으로 받은 상태값이나, 논리 연산의 결과가 `true`면 해당 태그를 렌더링 해주고 `false`인 경우 렌더링하지 않는다.  
`v-else`는 반대로 `v-if`의 값이 `false`인 경우에만 태그를 렌더링하는 역할을 한다.

### 렌더링은 되는데 숨기고 싶으면?
`v-if` 나 `v-else` 디렉티브를 통한 조건 출력은  
아예 dom 요소가 브라우저에 렌더링 되거나 되지 않게 해주는 것이다.  
그런데 만약 브라우저에 렌더링은 되지만, 보이지만 않게 하고 싶다면 어떻게 해야할까?
  
그럴 때 쓰는 것이 `v-show`!  
v-show 의 값으로 조건문을 넣어주면, 조건문이 성립할 때는 화면에 보이지만  
성립하지 않을 때는 스타일 속성의 값으로 `display:none`이 추가가 되어 화면에서만 보이지 않게 처리된다.

```html
<div id="app_if">
	<h3>당신의 지금 상태는?</h3>
	<p v-if="mystatus =='good'">매우 좋아 :)</p>
	<p v-else>좋지 않아 :(</p>
	<p v-show="myStatus=='good'">이도저도 모르겠다~</p>
</div>
```

## 5. 반복 렌더링 v-for
`v-for` 디렉티브는 javascript의 for-in문과 비슷하게 동작한다.  
배열이나 객체 형태의 데이터를 토대로 반복적으로 렌더링을 수행할 때 활용된다.  
  
```javascript
var app_for = new Vue({ 
	el: '#app_for',
	data: {
		studyList: [
			{ object: "Vue.js", level: "★★"},
			{ object: "Node.js", level: "★★★★"},
			{ object: "Python", level: "★★★"},
			{ object: "Three.js", level: "★★★★"}
		] 
	}
 });
```
vue 인스턴스 안에 있는 객체 형태의 데이터 `studyList`를 토대로 공부 목록을 출력하고자 한다.  
`v-for` 디렉티브의 기본 문법은 다음과 같다.

```
<tagName v-for="item in dataName"> {{ item }} </tagName>
```
dataName이라는 데이터 목록에서 각 요소가 item 로 호출되어서  
item의 개수만큼 태그가 반복되어 렌더링 된다.  
이 문법을 활용해서 위의 예제를 코드로 작성해보면 아래와 같다.

```html
<div id="app_for">
	<h2>내가 공부해야 할 것들</h2>
	<ul>
		<li v-for="item in studyList"> {{ item }} </li>
	</ul>
</div>
```

여기서 item 객체를 통째로 출력하지 말고 가독성 있게 수정해보자!

```html
<div id="app_for">
	<h2>내가 공부해야 할 것들</h2>
	<ul>
		<li v-for="(item, i) in studyList"> {{ (i+1) }}번째 공부: {{ item.object }} (난이도 {{ item.level }}) </li>
	</ul>
</div>
```

추가적으로 vue는 각 요소만다 유니크한 속성값을 줄 것을 권유한다.  
배열안에 중복 값이 없다고 가정하고 과목명을 key 값으로 추가해준다. 

```html
<div id="app_for">
	<h2>내가 공부해야 할 것들</h2>
	<ul>
		<li v-for="(item, i) in studyList" :key="item.object"> {{ (i+1) }}번째 공부: {{ item.object }} (난이도 {{ item.level }}) </li>
	</ul>
</div>
```

### 결과물
* 1번째 공부: Vue.js (난이도 ★★) 
* 2번째 공부: Node.js (난이도 ★★★★) 
* 3번째 공부: Python (난이도 ★★★) 
* 4번째 공부: Three.js (난이도 ★★★★) 


## 5. 감시자, vue watch
vue 인스턴스의 속성 `data, methods, computed, watch...` 중 `watch`는 한국어로 *감시자*로 번역되는 속성이다.
watch 함수들은 어원과 동일하게 특정 값의 변화를 감시하고, 변화가 있을 경우 정의한 함수를 실행시킨다.

```html
<div id="app_watch">
	<h3>캔디 {{ myCandy }} 개</h3>
	<p>{{ desc }}</p>
	<button @click="eatCandy">캔디먹기</button>
</div>
```

```javascript
var app_watch = new Vue({ 
	el: '#app_watch',
	data: {
		desc: '캔디가 있습니다~',
		myCandy: 10
	},
	methods:{
		eatCandy(){
			console.log('냠냠');
			this.myCandy--;
		}
	},
	watch: {
		myCandy(newVal, oldVal){
			this.desc = '캔디의 개수가 ' + oldVal + '개에서 ' + newVal +'개가 되었습니다.' 
		}
	}
});
```
위의 예제에서 button을 클릭할 때 마다 변수 `myCandy`의 값이 변화한다.  
그리고 동일한 `myCandy`의 이름으로 정의된 `myCandy` *watch 함수*는  
`myCandy`의 값이 변화할 때 마다 실행되는 함수라고 할 수 있다.  
이 watch 함수는 해당 변수의 새로운 값(newVal)과 이전 값(oldVal)을 인자로 받는다.


## 6. vue의 라이프사이클
스크립트에서 vue 인스턴스를 선언하게되면 vue 인스턴스가 생성되는데,  
이 생성과정에서 vue 인스턴스는 미리 사전에 정의된 몇 단계의 과정을 거치게 된다.  
이를 라이프사이클(lifecycle)이라 한다!  
  
Vue는 각 라이프사이클 단계에서 사용자들이 어떠한 코드 작용,  
훅(hook)을 할 수 있도록 API를 제공해준다.  
vue의 라이프사이클은 크게 Create > Mount > Update > destroy 4 단계를 거친다.  
이 라이프사이클의 과정에서 사용할 수 있는 API 종류는 다음과 같다.  

#### vue lifeCycle hook API
* beforeCreate
* created
* beforeMount
* mounted
* beforeUpdate
* updated
* beforeDestroy
* destroyed

이 중 많이 사용하는 것을 실제로 예제로 사용해본다.

```javascript
var vm = new Vue({ 
	el: '#app',
	data: {
		title: 'vue life cycle'
	},
	beforeCreate (){ //가장 먼저 실행되는 훅. data와 event 등이 아직 세팅되지 않았다.
		console.log("can't use Data, events also this.$el");
	},
	created() { //vue 인스턴스가 생성됐다. data와 events가 활성화되어 접근할 수 있다. 그러나 아직 템플릿, 가상돔과는 마운트 및 렌더링이 진행되지 않았다. 돔에는 접근할 수 없다.
		console.log("can use Data, events! but not yet dom elements this.$el");
	},  
	beforeMount() { //첫 렌더링(출력)이 일어나기 직전 훅이다. 대부분의 경우 사용하지 않는 것이 좋다.
		console.log("this.$el doesn't exist yet, but it will soon!")
	},
	mounted() {//가상돔과 마운팅 되는 단계의 훅. 컴포넌트, 템플릿, 렌더링된 돔에 접근할 수 있다.
		console.log("can use this.$el!") 
		console.log(this.$el.textContent); 
	},
	updated() { //이 훅은 컴포넌트의 데이터가 변하여 재 렌더링이 일어난 후에 실행된다.
		console.log("just updated!")	
	}
});

```

* [지그재그의 개발 블로그](https://wormwlrm.github.io/2018/12/29/Understanding-Vue-Lifecycle-hooks.html)



