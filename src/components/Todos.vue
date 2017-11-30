<template>
  <section class="todoapp">
    <header class="header">
      <h1>Todos</h1>
      <input type="text" class="new-todo" placeholder="ajouter une tâche" v-model="newTodo" @keyup.enter="addTodo">
    </header>
    <div class="main">
      <input type="checkbox" class="toggle-all" v-model="allDone">
      <ul class="todo-list">
        <li class="todo" v-for="todo in filteredTodos" :class="{completed: todo.completed, editing: todo === editing}">
          <div class="view">
            <input type="checkbox" class="toggle" v-model="todo.completed">
            <label @dblclick="editTodo(todo)">{{ todo.name }}</label>
            <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
          </div>
          <input type="text" class="edit" v-model="todo.name" @keyup.enter="doneEdit" @blur="doneEdit">
        </li>
      </ul>
    </div>
    <div class="footer" v-show="hasTodos">
      <span class="todo-count">
        <strong>{{ remaining }}</strong> Tâches à faire
      </span>
      <ul class="filters">
        <li><a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">Toutes</a></li>
        <li><a href="#" :class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'">A faire</a></li>
        <li><a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">Faites</a></li>
      </ul>
      <button class="clear-completed" v-show="doneTodo" @click.prevent="deleteCompleted">Supp</button>
    </div>
  </section>
</template>

<script>
  export default {
    data () {
      return {
        todos: [{
          name: 'Tache de test',
          completed: false
        }],
        newTodo: '',
        filter: 'all',
        editing: null
      }
    },
    methods: {
      addTodo () {
        this.todos.push({
          completed: false,
          name: this.newTodo
        })
        this.newTodo = ''
      },
      deleteTodo (todo) {
        this.todos = this.todos.filter(i => i !== todo)
      },
      editTodo (todo) {
        this.editing = todo
      },
      doneEdit () {
        this.editing = null
      }
    },
    computed: {
      allDone: {
        get () {
          return this.remaining === 0
        },
        set (value) {
          this.todos.forEach((todo) => {
            todo.completed = value
          })
        }
      },
      remaining () {
        return this.todos.filter(todo => !todo.completed).length
      },
      filteredTodos () {
        if (this.filter === 'todo') {
          return this.todos.filter(todo => !todo.completed)
        } else if (this.filter === 'done') {
          return this.todos.filter(todo => todo.completed)
        }
        return this.todos
      },
      hasTodos () {
        return this.todos.length > 0
      },
      doneTodo () {
        return this.todos.filter(todo => todo.completed).length > 0
      },
      deleteCompleted () {
        this.todos = this.todos.filter(todo => !todo.completed)
      }
    }
  }
</script>

<style src="./todos.css"></style>


