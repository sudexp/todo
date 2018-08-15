<template>
  <div class="todo-item">
    <div class="todo-item-left">
      <input type="checkbox"
             v-model="completed"
             @change="doneEdit">
      <div v-if="!editing"
           @dblclick="editTodo"
           class="todo-item-label"
           :class="{ completed: completed }">
        {{ title }}
      </div>
      <input v-else
             class="todo-item-edit" 
             type="text" 
             v-model="title"
             @blur="doneEdit"
             @keyup.enter="doneEdit"
             @keyup.esc="cancelEdit"
             v-focus>
      <div class="remove-item" 
           @click="removeTodo(todo.id)">
        &times;
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'todo-item',
  props: {
    todo: {
      type: Object,
      required: true
    },
    checkAll: { 
      type: Boolean,
      required: true
    }
  },
  data() {
    return {
      id: this.todo.id,
      title: this.todo.title,
      completed: this.todo.completed,
      editing: this.todo.editing,
      beforeEditCache: ''
    }
  },
  directives: {
    focus: {
      inserted: function(el) {
        el.focus();
      }
    }
  },
  watch: {
    checkAll() {
      this.completed = this.checkAll ? true : this.todo.completed
    }
  },
  methods: {
    removeTodo(id) {
      this.$emit('removedTodo', id)
    },
    editTodo() {
      this.beforeEditCache = this.title;
      this.editing = true
    },
    doneEdit() {
      if (this.editing === false) {
        return;
      }
      if (this.title.trim() === '') {
        this.title = this.beforeEditCache;
      }
      this.editing = false;
      this.todo.title = this.title;
    },
    cancelEdit() {
      this.title = this.beforeEditCache,
      this.editing = false;
    }
  }
};
</script>
