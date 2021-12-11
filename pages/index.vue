<template>
  <div id="app">
    <div class="container">
      <div class="title">
        <div class="col-xs-12">
          <h1>{{ title }}</h1>
        </div>
      </div>
      <div class="input">
        <div class="col-xs-12">
          <form class="form-inline text-center" @submit.prevent>
            <input v-model="todotext" type="text" />
            <button @click="createTodo">Add</button>
          </form>
        </div>
      </div>
      <div class="list">
        <ol id="sort">
          <div v-for="(item, index) in listTodos" :key="index" class="item">
            <input
              :checked="item.completed"
              type="checkbox"
              @change="checkComplete(item.id)"
            />
            <p>{{ item.title }}</p>
            <button @click="deleteTodo(item.id)">Del</button>
          </div>
        </ol>
      </div>
    </div>
  </div>
</template>

<script>
import { v4 as uuidv4 } from 'uuid'
export default {
  data() {
    return {
      title: 'Todo List',
      todotext: '',
      todos: [],
      listTodos: [],
      storageKey: 'todo',
      examples: [
        {
          id: '1',
          title: 'Wake up at 5am',
          completed: true,
        },
        {
          id: '2',
          title: 'Learn how to use Vue.js',
          completed: false,
        },
        {
          id: '3',
          title: 'Drink coffee',
          completed: false,
        },
      ],
    }
  },
  mounted() {
    this.todos = JSON.parse(localStorage.getItem(this.storageKey)) || []

    if (!this.todos.length) {
      this.todos = this.examples
      this.setStorage()
    }

    this.listTodos = this.todos
  },
  methods: {
    createTodo() {
      const jsonDataToDo = {
        id: uuidv4(),
        title: this.todotext,
        completed: false,
      }
      this.todos = [...this.todos, jsonDataToDo]
      this.setStorage()
      this.todotext = ''
      this.showAll()
    },
    setStorage() {
      localStorage.setItem(this.storageKey, JSON.stringify(this.todos))
    },
    showAll() {
      this.listTodos = this.todos
    },
    deleteTodo(id) {
      this.todos = this.todos.filter((x) => x.id !== id)
      this.setStorage()
      this.showAll()
    },
    checkComplete(id) {
      this.todos = this.todos.map((x) => {
        if (x.id === id) x.completed = !x.completed
        return x
      })
      this.setStorage()
      this.showAll()
    },
  },
}
</script>

<style>
.container {
  text-align: center;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
  line-height: 2;
  font-size: 17px;
}

fieldset {
  margin: 15px 25px;
  padding: 0 25px 30px;
}

legend {
  font-weight: bold;
  font-size: 20px;
}

.item {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

button {
  margin-left: 50px;
}

.list {
  display: flex;
  justify-content: center;
}
</style>
