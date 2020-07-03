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
그런데 만약 여기서 h1 태그가 innerHTML 값을 최초에 출력받은 'current title'으로 유지하고 싶다면?
  
이런 경우를 위해 등장한 프로퍼티가 바로 `v-once` 프로퍼티다.  

```html
<div id="app_once">
	<h1 v-once>{{ title }}</h1>
	<h2>{{ title }}</h2>
	<p>{{ afterBinding() }}</p>
</div>
```

v-once 프로퍼티가 입력되면, 바인딩 된 태그는 HTML코드로 출력이 된 이후에  
어떤 후처리가 있더라도 _최초에 출력한 값_을 유지시켜준다.  

### 실행결과
h1 태그에는 최초 설정 값인 'current title'을,
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

결과는? 변수에 담겨 있던 HTMl 태그가 돔 요소가 아닌 문자열로 그대로 출력되어 버린다.
이때를 위해 사용하는 디렉티브가 바로 `v-html`!

### v-html 을 사용해보자
```html
<div id="app_html">
	<p v-html="aTag"></p>
</div>
```
결과: 정상적으로 p 태그 안에 a 태그가 삽입되었다!

