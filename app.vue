<template>
  <div class="container">
    <h1>TODOアプリ</h1>
    <div class="todo-input-container">
      <input
        v-model="newTodo"
        type="text"
        placeholder="新しいTODOを追加"
        class="todo-input"
      />
      <button @click="addTodo" class="add-button">追加</button>
    </div>
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
      console.log("追加するよん")
      if (this.newTodo.trim()) {
        this.todos.push({ text: this.newTodo, completed: false });
        this.newTodo = ''; // 入力後は空にする
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
.todo-input-container {
  display: flex;
  gap: 10px;
}
.todo-input {
  width: 100%;
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #ccc;
}
.add-button {
  padding: 10px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
.add-button:hover {
  background-color: #45a049;
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
