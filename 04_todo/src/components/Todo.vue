<template>
  <button class="list-group-item text-left" @click="clickButton()">
      {{ label }}
  </button>
</template>
<script>
  export default {
    props:['label'],
    methods: {
      clickButton(){
        this.$emit('componentClick'); // button 태그는 clickButton() 메소드와 바인딩 되어있다.
                                    // 그리고 이 메소드는 외부로 componentClick 라는 이벤트를 발생시킨다. 
                                    // 이렇게 외부로 이벤트를 발생키셔야 할 때 $emit() 메소드를 사용해준다.
       }
    }
  };
</script>
