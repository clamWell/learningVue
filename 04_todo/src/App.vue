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

import Todo from './components/Todo'; //todo button을 외부 컴포넌트로 분리, 모듈화 시켜놓았다. 이를 임포팅 시켜준다.
export default {
    name: 'app',
	data(){
		return {
			userInput:'',
			todoList:[],
            currentState: 'active'
		};
	},
    computed: { // methods 가 아닌 computed 객체 안에 activeTodoList 함수를 위치시킨다. 이렇게 computed안에 선언해줄 경우 html 코드에서 아래 함수를 외부 변수처럼 사용할 수 있게 된다.
        // 이렇게 computed안에 선언해줄 경우 html 코드에서 아래 함수를 외부 변수처럼 사용할 수 있게 된다. 클래스의 getter함수처럼 동작한다.
        activeTodoList(){ //목록의 상태 설정과 일치한 목록만 보여주기 위한 filter 함수 적용
            return this.todoList.filter( todo => this.currentState==='all' || todo.state === this.currentState );
        }
    },
	methods:{
        changeCurrentState(state){ //넘겨 받은 상태로 todo 목록의 상태 설정을 바꿔준다. default 는 active.
            this.currentState = state;
        },
        addNewTodo(){ // 사용자 입력값을 데이터 객체로 받아 추가해주는 메소드
            if(this.userInput !== ""){
                this.todoList.push({
                    label: this.userInput,
                    state: 'active'
                });
                this.userInput = ''; //배열에 데이터 객체를 추가한 후에는 사용자 입력 변수를 초기화 시켜준다.
            }
        },
        toggleTodoStae(todo){ //인자로 받은 todo객체의 state 속성값을 toggling 해주는 메소드
            todo.state = todo.state === 'active'? 'done' : 'active' ;
        },
        checkActive(todo){ //인자로 받은 todo객체가 active 상태이면 true 반환
          if( todo.state === 'active') return true;
        }
    },
	components: {
        Todo // 컴포넌트를 선언해준다.
	}
}


</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.container { }
.text-center { text-align: center; }
.form-control {}

</style>
