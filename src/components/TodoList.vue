<template>
  <div class="hello">
      <h1 v-html="title"></h1>
      <input v-model="newItem" v-on:keyup.enter="addNew" placeholder="write to add">
      <ul>
        <li v-for="item in items" v-bind:class="{finished:item.isFinished}" v-on:click="toggleFinish(item)">
          {{item.label}}
        </li>
      </ul>
  </div>
</template>

<script>
var STORAGE_KEY = 'todos-vuejs'
var todoStorage = {
  fetch: function () {
    var todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]')
    todos.forEach(function (todo, index) {
      todo.id = index
    })
    todoStorage.uid = todos.length
    return todos
  },
  save: function (todos) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(todos))
  }
}
export default {
  name: 'TodoList',
  data () {
    return {
      title: 'This is a Todo List',
      newItem: '',
      items: todoStorage.fetch()
    }
  },
  methods: {
    toggleFinish: function (item) {
      item.isFinished = !item.isFinished
    },
    addNew: function () {
      this.items.push({
        label: this.newItem,
        isFinished: false
      })
      this.newItem = ''
    }
  },
  watch: {
    items: {
      handler: function (items) {
        todoStorage.save(items)
      },
      deep: true
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
.finished {
  text-decoration: underline;
}
</style>
