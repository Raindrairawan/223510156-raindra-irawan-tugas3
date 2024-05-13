<template>
  <div class="todo-list">
    <h1>To-Do List</h1>
    <form @submit.prevent="addTask">
      <input v-model="newTask" placeholder="Add a new task">
      <button type="submit">Add</button>
    </form>
    <ul>
      <li v-for="todo in todos" :key="todo.id" class="todo-item">
        <label v-if="!todo.editing" class="todo-item-label">
          <input
            type="checkbox"
            v-model="todo.completed"
            @change="updateTodo(todo)"
            class="todo-item__checkbox">
          {{todo.text}}
        </label>
        <button
          @click="deleteTodo(todo)"
          class="todo-button">
          <img src="./assets/trash.svg" alt="Delete todo">
        </button>
        <button
          v-if="!todo.editing"
          @click="editTodo(todo)"
          class="todo-button">
          <img src="./assets/pencil.svg" alt="Edit todo">
        </button>
        <input
          v-else
          v-model="todo.text"
          @keyup.enter="updateTodoText(todo)"
          @keyup.esc="cancelEdit(todo)"
          class="todo-item__edit"
          type="text">
      </li>
    </ul>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        newTask: '',
        todos: [
          {id: 1, text: 'Buy milk', completed: false, editing: false},
          {id: 2, text: 'Eat pizza', completed: false, editing: false},
          {id: 3, text: 'Update tutorial', completed: true, editing: false},
          {id: 4, text: 'Study Vue', completed: false, editing: false},
          {id: 5, text: 'Go kayaking', completed: true, editing: false}
        ]
      }
    },
    methods: {
      addTask() {
        if (this.newTask.trim() !== '') {
          this.todos.push({
            id: Date.now(),
            text: this.newTask.trim(),
            completed: false,
            editing: false
          });
          this.newTask = '';
        }
      },
      deleteTodo(todo) {
        this.todos = this.todos.filter(t => t !== todo);
      },
      editTodo(todo) {
        todo.editing = true;
      },
      updateTodoText(todo) {
        if (todo.text.trim() === '') {
          this.deleteTodo(todo);
          return;
        }
        todo.editing = false;
      },
      cancelEdit(todo) {
        todo.editing = false;
      },
      updateTodo(todo) {
        const index = this.todos.findIndex(t => t.id === todo.id);
        this.todos[index].completed = todo.completed;
      }
    }
  }
</script>

<style scoped>
  .todo-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 50px;
  }

  .todo-input {
    width: 300px;
    height: 40px;
    border: 1px solid #ccc;
    border-radius: 5px;
    padding: 10px;
    font-size: 16px;
    margin-bottom: 20px;
  }

  .todo-button {
    width: 100px;
    height: 40px;
    background-color: #4CAF50;
    color: rgb(255, 255, 255);
    border: none;
    border-radius: 5px;
    font-size: 16px;
    cursor: pointer;
    margin-right: 10px;
  }

  .todo-button:hover {
    background-color: #3e8e41;
  }

  .todo-list {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 30px;
  }

  .todo-item {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
    background-color: #757272;
    padding: 10px;
    border-radius: 5px;
  }

  .todo-text {
    margin-left: 10px;
    font-size: 16px;
  }

  .todo-delete-button {
    background-color: #f44336;
    color: white;
    border: none;
    border-radius: 50%;
    font-size: 16px;
    cursor: pointer;
    width: 30px;
    height: 30px;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-left: auto;
  }

  .todo-delete-button:hover {
    background-color: #da190b;
  }

  .completed {
    text-decoration: line-through;
  }
</style>