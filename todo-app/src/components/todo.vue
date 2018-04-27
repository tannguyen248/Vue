<template>
  <div class="todo">
    <input class="toggle" type="checkbox" v-model="todo.isCompleted">
    <label  v-show="!editing"
            @dblclick="editTodo()"
            :class="{ completed: todo.isCompleted }"
    >
            {{ todo.content }}
    </label>
    <input v-show="editing" class="edit" type="text"
        v-model="todo.content"
        @blur="doneEdit()"
        @keyup.enter="doneEdit()"
        @keyup.esc="doneEdit()" />
    <button class="destroy" v-on:click="$emit('remove-todo', todo)">X</button>
  </div>
</template>

<script>
export default {
  props: ['todo'],
  data() {
    return {
      editing: false,
    };
  },
  methods: {
    editTodo() {
      this.editing = true;
    },
    doneEdit() {
      this.editing = false;
    },
  },
};
</script>

<style>
  .completed {
    text-decoration: line-through;
  }
</style>
