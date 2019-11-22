<template>
  <div class="home">
    <h1>Todo List Django & Vue</h1>
    <TodoInput @createTodoEvent="createTodo" /> <!--@이벤트=이벤트발생시실행할methods-->
    <TodoList :todos="todos"/>
  </div>
</template>

<script>
// @ is an alias to /src
import TodoList from '@/components/TodoList.vue'
import TodoInput from '@/components/TodoInput.vue'
import router from '@/router'
import jwtDecode from 'jwt-decode' // bash에서 설치해준 패키지 이름만 써주면 됨
import axios from 'axios'

export default {
  name: 'home',
  components: {
    TodoInput,
    TodoList,
  },
  data() {
    return {
      todos: []
    }
  },
  methods: {
    loggedIn() {
      this.$session.start()
      if (!this.$session.has('jwt')) {
        router.push('/login')
      }
    },
    getTodos() {
      // axios => api/v1/users/현재접속한 user의 id를 얻기 위해 jwt을 디코딩해야함
      const token = this.$session.get('jwt')
      console.log(token)
      const user_id = jwtDecode(token).user_id // key-value pair로 PAYLOAD에 바로 접근 가능
      console.log(user_id)
      const options = {
        headers: {
          Authorization: 'JWT ' + token
        },
      }
      
      // 정보를 가져올 땐 get을 쓴다. Header정보도 같이 보내준다(두번째 인자로 넣어줌)
      axios.get(`http://127.0.0.1:8000/api/v1/users/${user_id}/`, options)
      .then(res => {
        console.log(res.data.todo_set)
        this.todos = res.data.todo_set
      }) // 데이터가 잘 도착했으면 callback 함수 실행
    },
    createTodo(title) {
      // getTodos()에서 한 거 복붙해도 됨. 
      const token = this.$session.get('jwt')
      const user_id = jwtDecode(token).user_id
      const options = {
        headers: {
          Authorization: 'JWT ' + token
        },
      }
      const data = new FormData()
      data.append('user', user_id)
      data.append('title', title)
      console.log(data)
      axios.post('http://127.0.0.1:8000/api/v1/todos/', data, options)
      .then(res => {
        console.log(res)
        this.todos.push(res.data)
      })
    }
  },

  // 8개의 life cycle hook
  mounted() {
    this.loggedIn() // 로그인 되었니?
    this.getTodos() // 데이터 가져왔니?
  },

}
</script>
