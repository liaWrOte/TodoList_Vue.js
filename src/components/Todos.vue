<template>
  <section class="todoapp">

    <header class="header">
      <i class="far fa-sun"></i>
      <h1 class="logo">Todos</h1>
      <p class="intro"><span class="introHello">Bonjour,</span> todoist </p>
      <span class="todoCount">
        <strong>
          Vous avez {{ remaining }} tâche<span v-if="todos.length > 1">s</span> aujourd'hui
          </strong>
      </span>

      <input type="text" class="newTodo" placeholder="Ajouter une tâche" v-model="newTodo" @keyup.enter="addTodo">
    </header>

    <div class="main">
      <p class="tasksIntro">Mes tâches</p>


      <ul class="todo-list">
        <div class="selectAll">
          <input type="checkbox" v-model="allDone" class="checkbox">
          <span class="selectAllLabel">Tout sélectionner</span>


        </div>
        <hr class="divider">
        <draggable v-model="filteredTodos" draggable=".todo">
        <li class="todo" v-for="todo in filteredTodos" :key="todo.name" :class="{completed: todo.completed, editing: todo === editing}">
          <div class="view">
            <div class="task">

            <input type="checkbox" v-model="todo.completed" class="checkbox">

            <span contenteditable="true" v-on:keydown.enter="doneEdit" :class="{striped : todo.completed}">
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
        </draggable>

      </ul>
    </div>
    <footer class="footer">
      <span class="footerLabel" >Filtrer mes tâches</span>
      <ul class="filter">
        <li><a href="#" class="filterItem" :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">Toutes<span v-if="todos.length > 0"> ({{ todos.length }})</span></a></li>
        <li><a href="#" class="filterItem" :class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'">A faire<span v-if="todos.length > 0"> ({{ remaining }})</span></a></li>
        <li><a  class="filterItem" href="#" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">Faites<span v-if="todos.length > 0"> ({{ completed }})</span></a></li>
      </ul>
      <button @click.prevent="deleteDone" v-show="completed" class="deleteDoneTasks">Supprimer les tâches terminées</button>
    </footer>
  </section>
</template>

<script>
import draggable from 'vuedraggable'

export default {
  components: {
    draggable
  },
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
    },
    deleteAll () {
      this.todos = [{
        name: '',
        completed: ''
      }]
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
