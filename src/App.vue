<template>
  <div id="app">
    <header>
      <h1 class="title" v-text="title"></h1>
      <input class="new" v-model="newTodo" @keyup.enter="addTodo" placeholder="what needs to be done?">
      <input class="toggleAll" :class="{isToggleAll:isToggleAll}" type="checkbox" v-model="isToggleAll" @click="toggleAll">
    </header>
    <div class="middle">
      <ul class="list" v-for="todo in todos" :key='todo.id'>
        <li class="item" @mouseover="showDelete(todo)" @mouseout="hideDelete(todo)">
          <input type="checkbox" class="toggle" @click="toggle(todo)" :checked="todo.isDone">
          <label class="text" :class="{finish:todo.isDone}" v-text="todo.text"></label>
          <button class="delete" :class="{show:todo.showDel}" @click="deleteItem(todo)">x</button>
        </li>
      </ul>
    </div>
    <div class="footer">
      <span class="counter"> items left</span>
      <ul class="filterBar">
        <li class="filters">
          <a class="filterBtn">All</a>
          <a class="filterBtn">Active</a>
          <a class="filterBtn">Completed</a>
        </li>
      </ul>
      <button class="clear">Clear Completed</button>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      title: 'todos',
      todos: [],
      newTodo: '',
      id: 0,
      isToggleAll: false,
    }
  },
  methods: {
    toggle: function (todo) {
      todo.isDone = !todo.isDone
      console.log(todo)
    },
    addTodo: function () {
      this.todos.push({
        id: this.id,
        text: this.newTodo,
        isDone: false,
        showDel: false,
      })
      this.newTodo = ''
      this.id++
    },
    toggleAll: function () {
      let isToggleAll = this.isToggleAll
      this.todos = this.todos.map((todo) => {
        todo.isDone = !isToggleAll
        return todo
      })
      this.isToggleAll = !this.isToggleAll
      console.log('========', this.todos)
    },
    showDelete: function (todo) {
      todo.showDel = true
    },
    hideDelete: function (todo) {
      todo.showDel = false
    },
    deleteItem: function (todo) {
      let id = todo.id
      this.todos = this.todos.filter((todo) => {
        todo.id !== id
      })
    }

  }
}


</script>

<style>
body {
  font: 14px "Helvetica Neue", Helvetica, Arial, sans-serif;
  line-height: 1.4em;
  background: #f5f5f5;
  color: #4d4d4d;
  min-width: 230px;
  max-width: 550px;
  margin: 0 auto;
  font-weight: 300;
  padding: 0;
}
#app {
  background: #fff;
  margin: 130px 0 40px 0;
  position: relative;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 25px 50px 0 rgba(0, 0, 0, 0.1);
}
.title {
  position: absolute;
  top: -155px;
  width: 100%;
  font-size: 100px;
  font-weight: 400;
  text-align: center;
  color: rgba(175, 47, 47, 0.15);
  text-rendering: optimizeLegibility;
}
.toggleAll {
  transform: rotate(90deg);
  outline: none;
  position: absolute;
  top: 15px;
  left: -12px;
  width: 60px;
  height: 34px;
  text-align: center;
  appearance: none;
}
.toggleAll:before {
  content: "❯";
  font-size: 22px;
  color: #e6e6e6;
  padding: 10px 27px 10px 27px;
}
.toggleAll:checked:before {
  color: #737373;
}
.new {
  padding: 16px 16px 16px 60px;
  border: none;
  background: rgba(0, 0, 0, 0.003);
  box-shadow: inset 0 -2px 1px rgba(0, 0, 0, 0.03);
  position: relative;
  margin: 0;
  width: 100%;
  font-size: 24px;
  font-style: italic;
  font-family: inherit;
  font-weight: 300;
  line-height: 1.4em;
  outline: none;
  color: black;
  box-sizing: border-box;
}
.middle {
  position: relative;
  z-index: 2;
  border-top: 1px solid #ededed;
}
.list {
  margin: 0;
  padding: 0;
  list-style: none;
}
.item {
  position: relative;
  font-size: 24px;
  border-bottom: 1px solid #ededed;
}
.toggle {
  height: 40px;
  text-align: center;
  width: 40px;
  outline: none;
  height: auto;
  position: absolute;
  top: 10px;
  bottom: 0;
  margin: auto 0;
  border: none;
  appearance: none;
}
.toggle:after {
  content: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="-10 -18 100 135"><circle cx="50" cy="50" r="50" fill="none" stroke="#ededed" stroke-width="3"/></svg>');
}
.toggle:checked:after {
  content: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="-10 -18 100 135"><circle cx="50" cy="50" r="50" fill="none" stroke="#bddad5" stroke-width="3"/><path fill="#5dc2af" d="M72 25L42 71 27 56l-4 4 20 20 34-52z"/></svg>');
}
.finish {
  text-decoration-line: line-through;
}
.text {
  white-space: pre-line;
  word-break: break-all;
  padding: 15px 60px 15px 15px;
  margin-left: 45px;
  display: block;
  line-height: 1.2;
  transition: color 0.4s;
}
.delete {
  position: absolute;
  top: 0;
  right: 10px;
  outline: none;
  display: none;
  bottom: 0;
  width: 40px;
  height: 40px;
  margin: auto 0;
  font-size: 30px;
  color: #cc9a9a;
  margin-bottom: 11px;
  border: none;
  background: none;
  transition: color 0.2s ease-out;
}
.show {
  display: block;
}
.delete:hover {
  color: #af5b5e;
}
.counter {
  float: left;
  text-align: left;
}
.footer {
  color: #777;
  padding: 10px 15px;
  height: 20px;
  text-align: center;
  border-top: 1px solid #e6e6e6;
}
.footer:before {
  content: "";
  position: absolute;
  right: 0;
  bottom: 0;
  left: 0;
  height: 50px;
  overflow: hidden;
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.2), 0 8px 0 -3px #f6f6f6,
    0 9px 1px -3px rgba(0, 0, 0, 0.2), 0 16px 0 -6px #f6f6f6,
    0 17px 2px -6px rgba(0, 0, 0, 0.2);
}
.filterBar {
  margin: 0;
  padding: 0;
  list-style: none;
  position: absolute;
  right: 0;
  left: 0;
  height: 19.2px;
}
.filterBtn {
  color: inherit;
  margin: 3px;
  padding: 3px;
  text-decoration: none;
  border: 1px solid transparent;
  border-radius: 3px;
  cursor: pointer;
}
.filterBtn:hover {
  border-color: rgba(175, 47, 47, 0.1);
}
.filterBtn:sele .filter {
  display: inline;
}
.clear {
  float: right;
  position: relative;
  line-height: 20px;
  cursor: pointer;
  margin: 0;
  padding: 0;
  border: 0;
  background: none;
  font-size: 100%;
  vertical-align: baseline;
  font-family: inherit;
  font-weight: inherit;
  color: inherit;
  outline: none;
}
.clear:hover {
  text-decoration: underline;
}
</style>
