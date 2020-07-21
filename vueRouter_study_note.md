# Vue router
* Vue.js에서 라우팅 기능을 구현할 수 있도록 제공하는 공식 라이브러리 Vue 라우터를 알아보자.
* 라우팅: 웹 페이지 내에서의 이동 방법. 한번의 서버 통신으로 html 페이지를 받은 후, 별도의 서버 통신 페이지 이동 없이 클라이언트의 라우팅을 통해 화면을 갱신하는 SPA에서 자주 사용된다.
* `Angular.js, React.js, Vue.js...` 등 모던 프론트엔드 프레임워크 기반의 웹앱들은 이러한 라우팅 기능을 통해 사용자에게 매끄러운 화면 전환을 제공한다.

--------------------------

## 1. Vue router 의 동작방식
vue 라우터의 화면 전환과 구성 방식에 대해 알아보자.

#### 📌 기본: 디렉토리 > 웹문서
만약 인터넷 브라우저 주소창에 **www.text.com/user/index.html** 라고 입력했다고 하면,
브라우저는 도메인 아래 root/user/ 에 해당하는 디렉토리에서 index.html 웹문서를 찾아서 이 웹 문서를 해독해 화면에 뿌려준다.

그렇다면 vue 라우터의 동작 방식은 어떨까?  

#### 📌 vue 라우터: 라우터이름 > 컴포넌트
브라우저 주소창에 **www.text.com/user** 라고 입력을 하면,  
Vue 라우터는 먼저 자신이 갖고 있는 라우터 목록중에 `user`라는 이름의 라우터가 있는지 찾는다.  
그리고 `user`라는 라우터와 매칭(맵핑)되어 있는 `user component`를 찾는다.  
`user component`는 여러가지 `component` 들의 조합으로 이루어져 있을 것이다.  
즉 이 여러개의 `component`로 조합된 화면이 `user component`, `user`라우터를 통해서  
Vue 라우터에 전달이 되고, Vue 라우터가 이 조합된 화면을 화면에 뿌려준다.

-----------------------------

## 2. vue 라우팅 살펴보기
#### 📌 환경설정
npm vue-cli 를 이용해 기본 환경 설정을 설치할 때 router 를 함께 설치해주면,  
디렉토리에 router.js 가 자동으로 생성이 된다.

터널에서 `npm run serve` 를 입력해 로컬 라이브 서버를 작동시킨다.
```
App running at:
- Local:   http://localhost:8080/
- Network: http://172.26.47.68:8080/
```
로컬(:8080)에서 이제 실시간으로 코드의 변화를 확인 할 수 있다.

#### 📌 구조, 디렉토리 살펴보기
최상단에 npm이 설치해준 .config, package.json 등의 환경설정 문서와 패키지들이 있다.
소스들이 위치해 있을 src 디렉토리를 보자. assets/, components/, plugins/, views/ 디렉토리가 있고 `App.vue`, `main.js`, `router.js`가 위치해 있다.
* └ assets: 이미지 소스 등
* └ ★components: Vue 컴포넌트들이 위치해있다.
* └ plugins: 개발의 편의를 위해 css 플러그인 `vuetify` 를 가져와 사용해준다.
* └ ★views: 마찬가지로 Vue 컴포넌트들이 위치해있다.

`main.js`에서는 vue를 활용해 최종 화면인 `App.vue` 컴포넌트를 `index.html`에 있는 `#app`태그에 바인딩해주었다. 이제 대부분의 작업은 `App.vue` 와 `router.js` 등에서 이루어진다.

`App.vue`의 html 태그 중에 `<router-view>`가 보이는데 이 태그가 바로 vue 라우터에게 조합한 컴퍼넌트, 태그 내용물을 뿌려 줄 공간임을 알려주는 태그다.

```
<router-view> <!--vue router 야 이곳에 뿌려줘 --></router-view>
```

이제 해당 태그 내부에는 라우팅을 통해 분기된 화면들이 보여질 예정이다.


#### 📌 router.js 살펴보기  
`router.js`는 vue 라우팅 기능을 구현하기 위한 소스들이 적혀있는 스크립트 파일이다.
파일을 열어보면 구조가 다음과 같다.

```javascript
import Vue from 'vue'
import Router from 'vue-router'
import Home from './views/Home.vue' //views 아래 home.vue 를 임포트

Vue.use(Router)

//component의 값을 상수 About 에 함수로 할당
const About = () => {
    return import(/* webpackChunkName: "about" */ './views/About.vue')
}

//임포트 해온 router 객체를 활용해 새로운 라우터 하나를 정의한 후
//이 안에 있는 객체들을 외부로 익스포팅 하겠다.
export default new Router({
  mode: 'history',
  base: process.env.BASE_URL,
  routes: [
    {
      path: '/', //주소, 위치
      name: 'home', //이름
      component: Home // 주소와 연결이 되는 컴퍼넌트. 위에서 임포팅 해온 HOme 컴포넌트가 여기서 사용되고 있음. 즉 이 path가 입력 되었을 때는 이 컴포넌트를 뿌려줘! 라고 추론해 볼 수 있음.
    },
    {
      path: '/about',
      name: 'about',
      //component: () => import(/* webpackChunkName: "about" */ './views/About.vue')
      component: About
    }
  ]
})


```
상단에서 Vue, Router 등의 기본 모듈과, vue 컴포넌트를 임포트해온다.
라우터를 사용하겠다고 선언해주고, 하단에서 Router 인스턴스를 생성, 작성해준다.
```javascript
Vue.use(Router) //사용 선언
export default new Router({
    ~~~~blah blah~~~~
})
```

각각의 라우트 객체 요소들은 **path, name, component** 세가지로 구성이 되어있다.
```javascript
{
  path: '/', //주소, 위치
  name: 'home', //이름
  component: Home // 주소와 연결이 되는 컴퍼넌트. 위 path가 입력 되었을 때는 이 컴포넌트를 뿌려줘!
},
```

🔔여기서 포인트!

임포팅 후 `Home` 변수에 할당된 home 컴포넌트와 달리 about 컴포넌트는 `About()` 함수의 리턴 값으로 할당이 되어있다. 차이는 뭘까? vue 컴포넌트를 함수의 리턴 값으로 할당하게 되면 vue 라우터가 제공하는 lazy-loaded 기능이 구현된다.

만약 함수로 호출하지 않고 vue 컴포넌트를 그냥 import 해주면 vue 라우터는 선언된 routes 객체들을 모두 불러와서 들고 있어야한다. 즉 로딩시간이 길어진다.

반면 about 컴포넌트는 함수로 호출이 되었기 때문에 페이지가 로딩이 되었을 때도 아직 불러와지지 않다가 주소창에 about이 입력되면 그제서야 함수를 통해 호출이 된다. 이러한 방식을 통해 이후에도 vue 라우터는 주소값이 바뀌면 필요한 컴포넌트들을 그때 그때 불러와 사용할 수 있게 된다. 결과적으로 훨씬 더 빠르게 화면을 로드하는 것이 가능해진다.

#### 📌 router 의 작동
이제 실제 로컬버서로 가서 주소창에 세부 주소를 입력해보자!
localhost:8080/home 을 입력하면  `<router-view>`태그 내부에 home 컴포넌트에 해당하는 화면이,
localhost:8080/about을 입력하면 about 컴포넌트에 해당하는 화면이 보임을 알 수 있다.

-----------------------------

## 3. vue 라우터에 접근하기

#### 📌 $router
다음으로 만약 네비게이션에서 클릭을 통해 vue 라우터 주소(path)를 변경해주고 싶으면 어떡할까? vue 라우터에 접근해 조작이 필요할 때는 `$router` 접근자를 사용한다.

그리고 router의 주소창을 바꾸고 싶을때는 `.push()` 메소드를 이용한다. router에 어떤 값을 밀어넣겠다는 의미다.

```html
<v-list-tile @click="$router.push({name:'/home'})">
  <v-list-tile-content>
    <v-list-tile-title>Home</v-list-tile-title>
  </v-list-tile-content>
</v-list-tile>
<v-list-tile @click="$router.push({name:'/about'})">
  <v-list-tile-content>
    <v-list-tile-title>Home</v-list-tile-title>
  </v-list-tile-content>
</v-list-tile>
```

vue 이벤트리스너 `@`를 통해 리스트 아이템을 클릭하면 vue router에 새로운 name 객체를 전달하도록 한다. 물론 이때 객체 형태가 아닌 value 값만 써줘도 작동한다.
```JavaScript
$router.push('/home')
```

그럼에도 굳이 객체형태로 작성을 해주는 이유는,
이렇게 객체형태로 쓰게 될 경우 추후 name 값을 라우터에 전달할 때 쿼리나, 파라미터 값도 같이 전달을 할 수 있기 때문이다.

```JavaScript
@click="$router.push({name:'/about'}, query: {}, params: {})"
```

#### 📌 router-link
 <router-link></router-link> 태그를 써주면 vue 라우터는 a태그, 링크를 생성해 준다.
 ```html
 <router-link :to="{ name: 'home'}">
      <p>이걸 클릭하면 home으로 이동</p>
 </router-link>
 ```
router-link 태그에서 `v-bind:to` 디렉티브의 값으로 새로운 name 객체를 전달해줄 수 있다.
이렇게 작성된 태그는 렌더링된 화면에서는
```html
<a href="/" class="router-link-active"><p>이걸 클릭하면 home으로 이동</p></a>
```
위에 처럼 a태그로 변환되어서 보인다.

router-link 태그를 이용해 기존의 네비게이션 코드 부분을 수정해준다.
```html
<v-list-tile router :to="{name: 'home'}" exact>
  <v-list-tile-content>
    <v-list-tile-title>Home</v-list-tile-title>
  </v-list-tile-content>
</v-list-tile>
<v-list-tile router :to="{name: 'about'}" exact>
  <v-list-tile-content>
    <v-list-tile-title>About</v-list-tile-title>
  </v-list-tile-content>
</v-list-tile>
```
🔔 라우터 링크에 부여된 exact 속성
라우터 링크에 exact 속성 값을 주게되면 예를들어 about의 경우,
주소창 path가 '/about' 일때 about 라우팅만 동작, '/'를 가리키는 'home'은 동작하지 않음.
만약 exact 속성 값이 없으면, '/about'의 path 값에 '/'도 포함되게 된다.

-------------------------

## 4. history mode vs hash mode
vue 라우터의 디폴트 모드는 `hash` 모드다.
따로 mode 설정을 해주지 않을 경우 `hash` 모드로 동작한다.

`hash` 모드에서는 기본 home 주소가 다음 처럼 `#`뒤에 위치한다.
```
http://localhost:8080/#/
http://localhost:8080/#/about/
```

그러나 일반적으로 웹의 주소는 이렇게 생기지 않았다..🤨
그래서 vue router가 제공하는 모드가 `history` 모드.
다음과 같이 `history` 모드를 설정해주면 우리에게 익숙한 형태로 주소를 분기할 수 있다.
```javascript
export default new Router({
  mode: 'history'
})
```
