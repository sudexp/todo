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
    };
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
      if (this.checkAll) {
        this.comleted = true;
      } else {
        this.completed = this.todo.completed;
      }
    }
  },
  methods: {
    removeTodo(id) {
      console.log('TodoItem.removeTodo ', id)
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
      console.log('doneEdit ', this.title)
      if (this.title.trim() === '') {
        this.title = this.beforeEditCache;
      }
      this.editing = false;
      this.todo.title = this.title;
      // this.$emit('finishedEdit', {
      //   index: this.index,
      //   todo: {
      //     id: this.id,
      //     title: this.title,
      //     completed: this.completed,
      //     editing: this.editing
      //   }
      // })
    },
    cancelEdit() {
      this.title = this.beforeEditCache,
      this.editing = false;
    }
  }
};
</script>
