<script setup>
import { ref, reactive, computed, onMounted } from "vue";
import List from './components/List.vue';

const todo = ref("");
const todos = reactive({
  list: [],
});

onMounted(() => {
  const savedTodos = localStorage.getItem("todos");
  todos.list = savedTodos ? JSON.parse(savedTodos) : [];
});

const totalTodo = computed(() => {
  return todos.list.length;
})

const saveToLocalStorage = () => {
  localStorage.setItem("todos", JSON.stringify(todos.list));
};

const addTodo = () => {
  todos.list.unshift({
    activity: todo.value,
    isDone: false,
  });
  todo.value = "";
  saveToLocalStorage();
};

const deleteTodo = (todoIndex) => {
  todos.list = todos.list.filter((item, index) => {
    if (index != todoIndex) {
      return item;
    }
  });

  saveToLocalStorage();
};

const doneTodo = (todoIndex) => {
  todos.list = todos.list.filter((item, index) => {
    if (index == todoIndex) {
      item.isDone = true;
    }

    return item;
  });

  saveToLocalStorage();
}
</script>

<template>
  <div class="min-h-screen bg-gray-100 flex items-center justify-center p-2 mx-auto">
    <div class="bg-white p-8 sm:p-16 rounded-lg shadow-lg w-full max-w-md">
      <h1 class="text-2xl font-bold mb-4 text-center text-gray-500">Todo List</h1>
      <div class="flex mb-4">
        <input
          v-model="todo"
          type="text"
          placeholder="Add new todo..."
          class="flex-1 border rounded-l-lg p-2 focus:outline-none focus:ring focus:border-blue-300 text-gray-500"
        />
        <button
          @click="addTodo"
          class="bg-blue-500 text-white p-2 rounded-r-lg hover:bg-blue-600 transition"
        >
          Add
        </button>
      </div>
      <p class="text-gray-600 mb-4">Total todos: {{ totalTodo }}</p>
      <List :todos="todos.list" @deleteTodo="deleteTodo" @doneTodo="doneTodo" />
    </div>
  </div>
        <div class="mb-4 text-center text-gray-500 text-sm">Created by adearyabimantara</div>
</template>
