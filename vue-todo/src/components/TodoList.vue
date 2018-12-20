<template lang="html">
  <section>
    <transition-group name="list" tag="ul">
      <!-- <li v-for="(item, index) in todoItems" class="shadow"> -->

      <!-- Reactivity(2): TodoList -->
      <li v-for="(item, index) in propsdata" v-bind:key="item" class="shadow">
        <i class="checkBtn fas fa-check" aria-hidden="true"></i>
        <!-- {{ item != 'loglevel:webpack-dev-server' ? item : '' }} -->
        {{ item }}
        <!-- <span class="removeBtn" type="button" v-on:click="removeTodo"> -->
        <span class="removeBtn" type="button" v-on:click="removeTodo(item, index)">
          <i class="far fa-trash-alt" aria-hidden="true"></i>
        </span>
      </li>
    </transition-group>
  </section>
</template>

<script>
export default {
  // data() {
  //   return {
  //     todoItems: []
  //   }
  // },
  // created() {
  //   if (localStorage.length > 0) {
  //     for (var i = 0; i < localStorage.length; i++) {
  //       this.todoItems.push(localStorage.key(i));
  //     }
  //   }
  // },

  // Reactivity(2): TodoList
  props: ['propsdata'],

  methods: {
    removeTodo(item, index) {
      // localStorage.removeItem(item);
      // this.todoItems.splice(index, 1);

      // Reactivity(4): TodoList(removeTodo)
      this.$emit('remove-todo', item, index)
    }
  }

}
</script>

<style lang="css" scoped>
  ul {
    list-style-type: none;
    padding-left: 0px;
    margin-top: 0;
    text-align: left;
  }
  li {
    display: flex;
    min-height: 50px;
    height: 50px;
    line-height: 50px;
    margin: 0.5rem 0;
    padding: 0 0.9rem;
    background: white;
    border-radius: 5px;
  }
  .checkBtn {
    line-height: 45px;
    color: #62acde;
    margin-right: 5px;
  }
  .removeBtn {
    margin-left: auto;
    color: #de4343;
  }
  /* Vue Animation */
  .list-enter-active, .list-leave-active {
    transition: all 1s;
  }
  .list-enter, .list-leave-to {
    opacity: 0;
    transform: translateY(30px);
  }

</style>
