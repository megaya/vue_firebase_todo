<template>
  <div id="app">
    <h2>タスク</h2>
    <div>
      <input type="text" v-model="newTodoName">
      <button type="submit" v-on:click="createTodo()">タスク作成</button>
    </div>
    <ul>
      <li><button type="submit" v-on:click="showTodoType = 'all'">すべて</button></li>
      <li><button type="submit" v-on:click="showTodoType = 'active'">未完タスク一覧</button></li>
      <li><button type="submit" v-on:click="showTodoType = 'complete'">完了タスク一覧</button></li>
    </ul>
    <ul v-for="(todo, key) in filteredTodos">
      <li><input class="toggle" type="checkbox" v-model="todo.isComplete" v-on:click="updateIsCompleteTodo(todo, key)">{{ todo.name }}</li>
      <button type="submit" v-on:click="deleteTodo(key)">削除</button>
    </ul>
  </div>
</template>

<script>
import firebase from 'firebase'

export default {
  name: 'App',
  created: function() {
    this.database = firebase.database();
    this.todosRef = this.database.ref('todos');

    var _this = this;
    this.todosRef.on('value', function(snapshot) {
      _this.todos = snapshot.val(); // データに変化が起きたときに再取得する
    });
  },
  computed: {
    filteredTodos: function () {
      if (this.showTodoType == 'all') {
        return this.todos;
      } else {
        var showIsComplete = false;
        if (this.showTodoType == 'complete') { showIsComplete = true }
        var filterTodos = {};
        for (var key in this.todos) {
          var todo = this.todos[key];
          if (todo.isComplete == showIsComplete) { filterTodos[key] = todo; }
        }
        return filterTodos;
      }
    }
  },
  methods: {
    createTodo: function() {
      if (this.newTodoName == "") { return; }
      this.todosRef.push({
        name: this.newTodoName,
        isComplete: false,
      })
      this.newTodoName = "";
    },
    updateIsCompleteTodo: function (todo, key) {
      todo.isComplete = !todo.isComplete
      var updates = {};
      updates['/todos/' + key] = todo;
      this.database.ref().update(updates);
    },
    deleteTodo: function (key) {
      this.database.ref('todos').child(key).remove();
    },
  },

  data () {
    return {
      database: null,
      todosRef: null,
      newTodoName: '',
      showTodoType: 'all',
      todos: []
    }
  }
}
</script>

<style>
</style>
