<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  todos: {
    type: Array,
    default: () => []
  }
});

const emit = defineEmits();

const deleteTODO = (index) => {
  emit('deleteTodo', index);
};

const done = (index) => {
  emit('doneTodo', index);
};
</script>

<template>
  <ul class="space-y-3">
    <li
      v-for="(todo, index) in todos"
      :key="index"
      class="flex items-center justify-between p-3 border rounded-lg text-gray-500"
      :class="{'bg-gray-200': todo.isDone, 'bg-gray-50': !todo.isDone}"
    >
      <div class="flex items-center">
        <input
          type="checkbox"
          :checked="todo.isDone"
          @change="done(index)"
          class="mr-3 h-5 w-5 text-blue-500"
        />
        <span :class="{'line-through text-gray-500': todo.isDone}">{{ todo.activity }}</span>
      </div>
      <button
        @click="deleteTODO(index)"
        class="text-red-500 hover:text-red-700 transition"
      >
        Delete
      </button>
    </li>
  </ul>
</template>