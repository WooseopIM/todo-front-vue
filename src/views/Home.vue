<template>
  <div class="home">
    <h1>Todo List Django & Vue</h1>
    <TodoInput />
    <TodoList :todos="todos"/>
  </div>
</template>

<script>
// @ is an alias to /src
import TodoList from '@/components/TodoList.vue'
import TodoInput from '@/components/TodoInput.vue'

export default {
  name: 'home',
  components: {
    TodoInput,
    TodoList,
  },
  data() {
    return {
      todos: [
        {id:1, title: 'Django DRF로 로그인 API 구현'},
        {id:2, title: 'JWT 활용한 세션 구현'},
        {id:3, title: 'Todo 관련 API 구현'},
        {id:4, title: 'VueX 활용한 Flux 아키텍처 적용'},
      ]
    }
  },
}
</script>
