# vue.js & todo-list

### 1. vue.js란? 
* MVVM 패턴에서 ViewModel 레이어에 해당하는 화면단 라이브러리이자 프레임워크.
* 실시간 데이터 바인딩을 제공하고, 화면의 구성 요소들을 컴포넌트 형태로 제공해준다.
* React, Angular에 이어서 모던 프론트 개발에서 가장 많이 사용되는 프레임워크다.

#### 2. 데이터 바인딩: vue 객체의 데이터와 HTML node의 연결

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
여기서는 #app 와 바인딩된 vue 인스턴스의 vueMessage 변수의 값이 리턴된다.  
이것이 Vue의 가장 기본적인 데이터 바인딩 방법이다.


#### 3. input 태그와 v-model: v-model 프로퍼티를 이용해 사용자 입력값 가져오기
프론트 개발을 하다보면 폼 태그를 이용해 사용자가 입력한 데이터 값을 가져와 저장하고,
이것을 다시 뷰단에 활용하는 작업을 많이 하게 된다.
일반적인 javascript 개발에서는 이 작업을 다음과 같은 코드로 수행한다.

* A. javascript 방식  
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


* B. Vue 방식
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


#### 4. 데이터로 목록 출력하기
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
