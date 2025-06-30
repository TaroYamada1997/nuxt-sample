<template>
  <div class="container">
    <h1>TODOアプリ</h1>
    <div class="todo-input-container">
      <input
        v-model="newTodo"
        type="text"
        placeholder="新しいTODOを追加"
        class="todo-input"
        @keyup.enter="addTodo"
      />
      <button @click="addTodo" class="add-button">追加</button>
    </div>
    
    <div class="kanban-board">
      <div class="kanban-column">
        <h2>未完了</h2>
        <ul class="todo-list" @dragover.prevent @drop="onDrop('todo')">
          <li
            v-for="(todo, index) in todoItems"
            :key="todo.id"
            class="todo-item"
            draggable="true"
            @dragstart="startDrag($event, todo)"
          >
            <div class="todo-content">
              <span>{{ todo.text }}</span>
            </div>
            <div class="todo-actions">
              <button @click="moveToInProgress(todo)" class="move-button">進行中へ</button>
              <button @click="deleteTodo(todo.id)" class="delete-button">削除</button>
            </div>
          </li>
        </ul>
      </div>
      
      <div class="kanban-column">
        <h2>進行中</h2>
        <ul class="todo-list" @dragover.prevent @drop="onDrop('inProgress')">
          <li
            v-for="(todo, index) in inProgressItems"
            :key="todo.id"
            class="todo-item in-progress"
            draggable="true"
            @dragstart="startDrag($event, todo)"
          >
            <div class="todo-content">
              <span>{{ todo.text }}</span>
            </div>
            <div class="todo-actions">
              <button @click="moveToCompleted(todo)" class="move-button">完了へ</button>
              <button @click="moveToTodo(todo)" class="move-button">未完了へ</button>
              <button @click="deleteTodo(todo.id)" class="delete-button">削除</button>
            </div>
          </li>
        </ul>
      </div>
      
      <div class="kanban-column">
        <h2>完了</h2>
        <ul class="todo-list" @dragover.prevent @drop="onDrop('completed')">
          <li
            v-for="(todo, index) in completedItems"
            :key="todo.id"
            class="todo-item completed"
            draggable="true"
            @dragstart="startDrag($event, todo)"
          >
            <div class="todo-content">
              <span>{{ todo.text }}</span>
            </div>
            <div class="todo-actions">
              <button @click="moveToInProgress(todo)" class="move-button">進行中へ</button>
              <button @click="deleteTodo(todo.id)" class="delete-button">削除</button>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: [],
      nextId: 1
    };
  },
  computed: {
    todoItems() {
      return this.todos.filter(todo => todo.status === 'todo');
    },
    inProgressItems() {
      return this.todos.filter(todo => todo.status === 'inProgress');
    },
    completedItems() {
      return this.todos.filter(todo => todo.status === 'completed');
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim()) {
        this.todos.push({ 
          id: this.nextId++,
          text: this.newTodo, 
          status: 'todo'
        });
        this.newTodo = '';
      }
    },
    deleteTodo(id) {
      const index = this.todos.findIndex(todo => todo.id === id);
      if (index !== -1) {
        this.todos.splice(index, 1);
      }
    },
    moveToInProgress(todo) {
      todo.status = 'inProgress';
    },
    moveToCompleted(todo) {
      todo.status = 'completed';
    },
    moveToTodo(todo) {
      todo.status = 'todo';
    },
    startDrag(event, todo) {
      event.dataTransfer.dropEffect = 'move';
      event.dataTransfer.effectAllowed = 'move';
      event.dataTransfer.setData('todoId', todo.id.toString());
    },
    onDrop(status) {
      const todoId = parseInt(event.dataTransfer.getData('todoId'));
      const todo = this.todos.find(todo => todo.id === todoId);
      if (todo) {
        todo.status = status;
      }
    }
  }
};
</script>

<style scoped>
.container {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}
.todo-input-container {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
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

.kanban-board {
  display: flex;
  gap: 20px;
  overflow-x: auto;
}

.kanban-column {
  flex: 1;
  min-width: 300px;
  background-color: #f4f5f7;
  border-radius: 5px;
  padding: 10px;
}

.kanban-column h2 {
  text-align: center;
  padding: 10px;
  margin: 0 0 10px 0;
  border-bottom: 1px solid #ddd;
}

.todo-list {
  list-style: none;
  padding: 0;
  min-height: 200px;
}

.todo-item {
  display: flex;
  flex-direction: column;
  margin-bottom: 10px;
  padding: 10px;
  border-radius: 5px;
  background-color: white;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12);
  cursor: grab;
}

.todo-item:active {
  cursor: grabbing;
}

.todo-content {
  margin-bottom: 10px;
}

.todo-actions {
  display: flex;
  gap: 5px;
  flex-wrap: wrap;
}

.move-button {
  background-color: #2196F3;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 3px;
  cursor: pointer;
  font-size: 12px;
}

.move-button:hover {
  background-color: #0b7dda;
}

.delete-button {
  background-color: #f44336;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 3px;
  cursor: pointer;
  font-size: 12px;
}

.delete-button:hover {
  background-color: #d32f2f;
}

.in-progress {
  border-left: 4px solid #ff9800;
}

.completed {
  border-left: 4px solid #4caf50;
  background-color: #f9f9f9;
}
</style>
