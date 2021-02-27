<template>
  <section class="todoapp">
    <header class="header">
      <h1 class="logo">Todos</h1>
      <p class="intro"><span class="introHello">Bonjour,</span> todoist </p>
      <span class="todoCount">
        <strong>
          Vous avez {{ remaining }} tâche aujourd'hui<span v-if="todos.length > 1">s</span>
          </strong>
      </span>

      <i class="fas fa-search"></i>
      <input type="text" class="newTodo" placeholder="Ajouter une tâche" v-model="newTodo" @keyup.enter="addTodo">
    </header>

    <div class="main">
      <p class="tasksIntro">Mes tâches</p>

      <ul class="todo-list">
        <li class="todo" v-for="todo in filteredTodos" :key="todo.name" :class="{completed: todo.completed, editing: todo === editing}">
          <div class="view">

            <div class="task">
              <div class="checkboxContainer">
            <input type="checkbox" v-model="todo.completed" class="checkbox">
            <span class="overlay" v-if="completed" @click="todo.completed">
              <svg
         xmlns="http://www.w3.org/2000/svg"
         width="24"
         height="24"
         viewBox="0 0 24 24"
         fill="none"
         stroke="currentColor"
         stroke-width="2"
         stroke-linecap="round"
         stroke-linejoin="round"
         stroke-dasharray="24"
         stroke-dashoffset="-24"
         class="icon">
              <polyline points="20 6 9 17 4 12"/>
          </svg>
        </span>
            </div>
            <span contenteditable="true" v-on:keydown.enter="doneEdit" class="taskItem">
              {{ todo.name}}
            </span>
            </div>
            <div class="editTask">
            <span class="destroy" @click.prevent="deleteTodo(todo)"><i class="far fa-minus-square"></i>
            </span>
            <span class="drag"><i class="fas fa-arrows-alt"></i></span>
            </div>
          </div>
        </li>

      </ul>
    </div>
    <footer class="footer" v-show="todos.length > 0">
      <input type="checkbox" v-model="allDone">
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

<style src="./styles/_reset.css"></style>
<style src="./styles/todos.css"></style>
