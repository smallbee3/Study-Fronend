<template lang="html">
  <div class="inputBox shadow">
    <!-- <input type="text" v-model="newTodoItem">
    <button v-on:click="addTodo">add</button> -->
    <input type="text" v-model="newTodoItem" placeholder="Type what you want to do"
    v-on:keyup.enter='addTodo'>
    <span class="addContainer" v-on:click="addTodo">
      <i class="addBtn fas fa-plus" aria-hidden="true"></i>
    </span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodoItem: ''
    }
  },
  methods: {
    addTodo() {
      // console.log(this.newTodoItem);
      if (this.newTodoItem.trim() !== "") {
        var value = this.newTodoItem.trim();
        localStorage.setItem(value, value);
        this.clearInput();
      }
    },
    clearInput() {
      this.newTodoItem = '';
    }
  }
}
</script>

<style lang="css" scoped>
  input:focus {
    outline: none;
  }
  .inputBox {
    background: white;
    height: 50px;
    line-height: 50px;
    border-radius: 5px;
  }
  .inputBox input {
    border-style: none;
    font-size: 0.9rem;
  }
  .addContainer {
    float: right;
    background: linear-gradient(to right, #6478FB, #8763FB);
    display: block;
    width: 3rem;
    border-radius: 0 5px 5px 0;
  }
  .addBtn {
    color: white;
    vertical-align: middle;
  }
</style>
