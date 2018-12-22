<template lang="html">
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:add-todo="addTodo"></TodoInput>
    <TodoList v-bind:propsdata="todoItems" v-on:remove-todo="removeTodo" v-on:edit-todo="editTodo"></TodoList>
    <TodoFooter v-on:clear-todo="clearTodo"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'

export default {
  // Reactivity(2): TodoList
  data() {
    return {
      todoItems: []
    }
  },
  created() {
    // if (localStorage.length > 0) {
    //   for (var i = 0; i < localStorage.length; i++) {
    //     this.todoItems.push(localStorage.key(i));
    //   }
    // }
    var allCookies = this.getAllCookies();
    if (allCookies.length > 0) {
      for (var i = 0; i < allCookies.length; i++) {
        this.todoItems.push(allCookies[i]);
      }
    }
  },
  methods:{
    // cookie
    setCookie(name,value,days) {
        var expires = "";
        if (days) {
            var date = new Date();
            date.setTime(date.getTime() + (days*24*60*60*1000));
            expires = "; expires=" + date.toUTCString();
        }
        document.cookie = name + "=" + (value || "")  + expires + "; path=/";
    },
    getCookie(name) {
        var nameEQ = name + "=";
        var ca = document.cookie.split(';');
        for(var i=0;i < ca.length;i++) {
            var c = ca[i];
            while (c.charAt(0)==' ') c = c.substring(1,c.length);
            if (c.indexOf(nameEQ) == 0) return c.substring(nameEQ.length,c.length);
        }
        return null;
    },
    getAllCookies() {
        var allCookies = [];
        var ca = document.cookie.split(';');
        for(var i=0;i < ca.length;i++) {
            var c = ca[i];
            while (c.charAt(0)==' ') c = c.substring(1,c.length);
            if (c.indexOf('csrftoken')=='-1') {
              allCookies.push(c.substring(0,c.indexOf('=')));
            }
        }
        return allCookies;
    },
    deleteCookie(name) {
      document.cookie = name+'=; Max-Age=-99999999;';
    },
    deleteAllCookies() {
        var ca = document.cookie.split(';');
        for(var i=0;i < ca.length;i++) {
            var c = ca[i];
            while (c.charAt(0)==' ') c = c.substring(1,c.length);
            if (c.indexOf('csrftoken')=='-1') {
              var name = c.substring(0,c.indexOf('='))
              document.cookie = name+'=; Max-Age=-99999999;';
            }
        }
    },

    // Reactivity(1): TodoInput - adding new item
    addTodo(value) {
      // This below code is the reason of changing the whole code p.160
      this.todoItems.push(value);

      // localStorage.setItem(value, value);
      this.setCookie(value, value, 7);
    },
    // Reactivity(4): TodoList(removeTodo)
    removeTodo(item, index) {
      this.deleteCookie(item);

      // localStorage.removeItem(item);
      this.todoItems.splice(index, 1);
    },
    // Reactivity(3): TodoFooter
    clearTodo() {
      // this.todoItems.splice(0, this.todoItems.length);
      this.todoItems = [];

      // localStorage.clear();
      this.deleteAllCookies();
    },
    editTodo(item, index) {
      var deleteItem = this.todoItems[index];

      // localStorage.removeItem(deleteItem);
      this.deleteCookie(deleteItem);
      this.todoItems.splice(index, 1);

      // localStorage.setItem(item, item);
      this.setCookie(item, item, 7);
      this.todoItems.push(item);
    }
  },
  components: {
    'TodoHeader': TodoHeader,
    'TodoInput': TodoInput,
    'TodoList': TodoList,
    'TodoFooter': TodoFooter
  }
}
</script>

<style lang="css">
  body {
    text-align: center;
    background-color: #F6F6F8;
  }
  input {
    border-style: groove;
    width: 200px;
  }
  button {
    border-style: groove;
  }
  .shadow {
    box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03)
  }
</style>





<!-- <template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>{{ msg }}</h1>
    <h2>Essential Links</h2>
    <ul>
      <li><a href="https://vuejs.org" target="_blank">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank">Twitter</a></li>
    </ul>
    <h2>Ecosystem</h2>
    <ul>
      <li><a href="http://router.vuejs.org/" target="_blank">vue-router</a></li>
      <li><a href="http://vuex.vuejs.org/" target="_blank">vuex</a></li>
      <li><a href="http://vue-loader.vuejs.org/" target="_blank">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank">awesome-vue</a></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
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

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style> -->
