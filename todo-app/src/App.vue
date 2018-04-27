<template>
  <div id="app">
    <input v-model="newTodo"  @keyup.enter="add" />
    <ul class="todos">
      <li
        is="todo"
        v-for="todo in filteredTodos" :todo="todo" :key="todo.id"
        @remove-todo="removeTodo">
      </li>
    </ul>
    <div class="footer" v-show="todos.length" v-cloak>
      <span class="todo-count">
        <strong>{{ remaining }}</strong> {{ remaining | pluralize }} left
      </span>
      <ul class="filters">
        <li>
          <a href="#/all" @click="visibility = 'all'"
              :class="{ selected: visibility == 'all' }">
            All
          </a>
        </li>
        <li>
          <a href="#/active" @click="visibility = 'active'"
              :class="{ selected: visibility == 'active' }">
            Active
          </a>
        </li>
        <li>
          <a href="#/completed" @click="visibility = 'completed'"
              :class="{ selected: visibility == 'completed' }">
            Completed
          </a>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import todo from './components/todo';

export default {
  name: 'App',
  components: {
    todo,
  },
  data() {
    return {
      newTodo: '',
      todos: [{ id: Date.now(), content: '1234', isCompleted: false, editing: false }],
      visibility: 'all',
    };
  },
  computed: {
    remaining() {
      return this.todos.filter(x => !x.isCompleted).length;
    },
    filteredTodos() {
      switch (this.visibility) {
        case 'active':
          return this.todos.filter(x => !x.isCompleted);
        case 'completed':
          return this.todos.filter(x => x.isCompleted);
        default:
          return this.todos;
      }
    },
  },
  methods: {
    add() {
      if (this.newTodo && this.newTodo.length > 0) {
        this.todos.push({ id: Date.now(), content: this.newTodo, isCompleted: false });
      }

      this.newTodo = '';
    },
    removeTodo(todoItem) {
      this.todos.splice(this.todos.indexOf(todoItem), 1);
    },
  },
  filters: {
    pluralize(remaining) {
      return remaining > 1 ? 'items' : 'item';
    },
  },
};
</script>
