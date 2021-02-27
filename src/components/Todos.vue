<template>
  <section class="todoapp">
    <header class="header">
      <h1>Todos</h1>
      <input type="text" class="new-todo" placeholder="Ajouter une tâche" v-model="newTodo" @keyup.enter="addTodo">
    </header>
    <div class="main">
      <input type="checkbox" v-model="allDone">
      <ul class="todo-list">
        <li class="todo" v-for="todo in filteredTodos" :key="todo.name" :class="{completed: todo.completed, editing: todo === editing}">
          <div class="view">
            <span class="drag">drag</span>
            <input type="checkbox" v-model="todo.completed">
            <span contenteditable="true" v-on:keydown.enter="doneEdit">
              {{ todo.name}}
            </span>
            <button class="destroy" @click.prevent="deleteTodo(todo)">x</button>
          </div>
        </li>

      </ul>
    </div>
    <footer class="footer" v-show="todos.length > 0">
      <span class="todo-count">
        <strong>
          Il vous reste {{ remaining }} tâche<span v-if="todos.length > 1">s</span>.
          </strong>
      </span>
      <ul>
        <li><a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">Toutes<span v-if="todos.length > 0"> ({{ todos.length }})</span></a></li>
        <li><a href="#" :class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'">A faire<span v-if="todos.length > 0"> ({{ remaining }})</span></a></li>
        <li><a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">Faites<span v-if="todos.length > 0"> ({{ completed }})</span></a></li>
      </ul>
      <button @click.prevent="deleteDone" v-show="completed">Supprimer les tâches terminées</button>
    </footer>
  </section>
</template>

<script>
export default {
  data () {
    return {
      todos: [{
        name: 'Tâche de test',
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
    deleteDone () {
      this.todos = this.todos.filter(i => i.completed === !true)
    },
    editTodo (todo) {
      this.editing = todo
    },
    doneEdit (e) {

      this.editing = null
      e.preventDefault()
    }
  },
  computed: {
    allDone: {
      get () {
        return this.remaining === 0
      },
      set (value) {
        this.todos.forEach(todo => {
          todo.completed = value
        })
      }
    },
    remaining () {
      return this.todos.filter(todo => !todo.completed).length
    },
    completed () {
      return this.todos.filter(todo => todo.completed).length
    },

    filteredTodos () {
      if (this.filter === 'todo') {
        return this.todos.filter(todo => !todo.completed)
      } else if (this.filter === 'done') {
        return this.todos.filter(todo => todo.completed)
      }
      return this.todos
    }
  }
}
</script>

<style src="./todos.css"></style>
