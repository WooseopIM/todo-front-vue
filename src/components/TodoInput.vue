<template>
  <div class="todo-input">
    <h2>Todo 입력하기</h2>
    <!--click과 enter 이벤트를 동시에 커버하자-->
    <!--prevent로 원래 하던 행위를 막아주고 정의해준 createTodo methods만 실행하게 만들기-->
    <form @submit.prevent="createTodo" class="input-group mb-3">
      <input v-model="title" type="text" class="form-control" placeholder="todo를 입력해주세요" />
      <button type="submit" class="btn btn-primary">+</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "TodoInput",
  data() {
    return {
      title: ""
    };
  },
  methods: {
    createTodo() {
      this.$emit("createTodoEvent", this.title);
      console.log('입력 전송')
      this.title = ''
    }
  }
};
</script>

<style>
</style>