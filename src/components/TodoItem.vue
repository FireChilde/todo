<template>
  <ul id="myUL">
    <template  v-if="todos.length == 0">
      <li>TodoList를 등록하세요.</li>
    </template>
    <template v-else>
      <li v-for="(item, index) in todos" :key="item.id" v-bind:class="{checked : item.isChecked}">
        <span v-on:click="checkItem(index)">{{item.title}}</span>
        <button v-on:click="removeItem(index)">X</button>
      </li>
    </template>
  </ul>
</template>

<script>
  export default {
    props: {
        todos: {
        }
    },
    methods: {
      removeItem(index) {
        this.todos.splice(index,1);
      },
      checkItem(index) {
        this.$emit("checkItem", index);
      }
    }
  }
</script>

<style>

</style>
