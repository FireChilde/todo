<template>
  <div class="todoList">
    <div class="header">
      <h2>My To Do List</h2>    
      <p>
        <input v-model="text" placeholder="할 일을 입력하세요..">
        <button class="addBtn" v-on:click="addTodo">click</button>
      </p>
    </div>
    <TodoItem v-bind:todos = "todos" v-on:checkItem="checkItem"></TodoItem>
  </div>
</template>

<script>
import TodoItem from './TodoItem.vue'

export default {
  name: 'TodoList',
  components: {
    TodoItem
  },
  data() {
    return {
      text: "",
      todos : [
        {
          id : 1,
          title : '첫 번째 할일',
          isChecked : false
        },
        {
          id : 2,
          title : '두 번째 할일',
          isChecked : true
        }
      ],
      nextId : 3
    }
  },
  methods: {
    addTodo() {
      if(this.text.length > 0){
        this.todos.push({id:this.nextId++,title:this.text,isChecked:false});
        this.text = "";
      }
      else{
        alert("일정을 입력 후 클릭 하세요");
      }
    },
    checkItem(index){
      this.todos[index].isChecked = !this.todos[index].isChecked;
    }
  }
}
</script>

<style scoped>

</style>
