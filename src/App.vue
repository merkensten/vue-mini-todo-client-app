<script setup lang="ts">
import { ref, reactive, computed } from 'vue';

type todoItemType = {
  id: number;
  text: string;
  isCompleted: boolean;
};

const todoItem = ref('');

const todoItems = reactive<todoItemType[]>([]);

const createTodoItem = (text: string): todoItemType => {
  return {
    id: todoItems.length + 1,
    text,
    isCompleted: false,
  };
};

const addTodo = (todoText: string) => {
  todoItems.push(createTodoItem(todoText));

  todoItem.value = '';
};

const removeTodo = (id: number) => {
  const index = todoItems.findIndex((todo) => todo.id === id);
  todoItems.splice(index, 1);
};

const sortedTodos = computed(() => {
  return todoItems.sort((a, b) => {
    if (a.isCompleted && !b.isCompleted) {
      return 1;
    }

    if (!a.isCompleted && b.isCompleted) {
      return -1;
    }

    return 0;
  });
});
</script>

<template>
  <div class="m-10 bg-slate-100 p-20 rounded-lg">
    <h1 class="text-4xl">Vue mini todo app</h1>
    <div>
      <div class="my-4">
        <form @submit.prevent="addTodo(todoItem)">
          <input
            class="w-full p-4 my-2 rounded-sm"
            v-model.trim="todoItem"
            type="text"
            placeholder="Add a todo..."
          />
          <button class="w-full p-4 my-2 rounded-sm bg-gray-800 text-white">
            Add todo
          </button>
        </form>
      </div>
      <h2 class="text-xl">Todos:</h2>
      <div>
        <ul class="flex flex-col gap-1">
          <li
            v-for="(todo, index) in sortedTodos"
            :key="index"
            class="my-2 p-4 rounded-lg bg-white shadow-md"
          >
            <div class="flex justify-between px-4 align-middle">
              <p class="my-auto text-xl">{{ todo.text }}</p>
              <div class="flex gap-4">
                <div>
                  <span v-if="todo.isCompleted"
                    ><button
                      @click="todo.isCompleted = false"
                      class="bg-gray-400 py-2 px-8 rounded-md text-black"
                    >
                      Reset
                    </button></span
                  >
                  <span v-else
                    ><button
                      @click="todo.isCompleted = true"
                      class="bg-gray-600 py-2 px-8 rounded-md text-white"
                    >
                      Complete
                    </button></span
                  >
                </div>
                <button @click="removeTodo(todo.id)" class="">
                  Delete todo
                </button>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
