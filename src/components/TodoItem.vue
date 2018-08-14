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
    }
  },
  data() {
    return {
      id: this.todo.id,
      title: this.todo.title,
      completed: this.todo.copmleted,
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
  methods: {
    removeTodo(id) {
      this.$emit('removedTodo', id)
    },
    editTodo() {
      this.beforeEditCache = this.title,
      todo.editing = true
    },
    doneEdit() {
      if (todo.title.trim() === '') {
        this.title = this.beforeEditCache;
      }
      this.editing = false;
      this.$emit('finishedEdit', {
        index: this.index,
        todo: {
          id: this.id,
          title: this.title,
          completed: this.completed,
          editing: this.editing
        }
      })
    },
    cancelEdit() {
      this.title = this.beforeEditCache,
      this.editing = false;
    }
  }
};
</script>

<style lang="scss">
.todo-item {
  margin-bottom: 10px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  animation-duration: 0.5s;
}

.remove-item {
  cursor: pointer;
  margin-left: 14px;
  &:hover {
    color: black;
  }
}

.todo-item-left {
  display: flex;
  align-items: center;
}
</style>
