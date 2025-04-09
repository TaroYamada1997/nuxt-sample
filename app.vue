<template>
  <div class="container">
    <h1>TODOアプリ</h1>
    <input
      v-model="newTodo"
      @keyup.enter="addTodo"
      type="text"
      placeholder="新しいTODOを追加"
      class="todo-input"
    />
    <ul class="todo-list">
      <li
        v-for="(todo, index) in todos"
        :key="index"
        class="todo-item"
      >
        <input
          type="checkbox"
          v-model="todo.completed"
          @change="toggleCompletion(index)"
        />
        <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
        <button @click="deleteTodo(index)">削除</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: [],
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim()) {
        this.todos.push({ text: this.newTodo, completed: false });
        this.newTodo = '';
      }
    },
    deleteTodo(index) {
      this.todos.splice(index, 1);
    },
    toggleCompletion(index) {
      this.todos[index].completed = !this.todos[index].completed;
    },
  },
};
</script>

<style scoped>
.container {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}
.todo-input {
  width: 100%;
  padding: 10px;
  margin-bottom: 20px;
  border-radius: 5px;
  border: 1px solid #ccc;
}
.todo-list {
  list-style: none;
  padding: 0;
}
.todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px 0;
  border-bottom: 1px solid #ddd;
}
.todo-item button {
  background-color: #f44336;
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
}
.todo-item button:hover {
  background-color: #d32f2f;
}
.completed {
  text-decoration: line-through;
  color: gray;
}
</style>
