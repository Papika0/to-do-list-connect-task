<template>
    <div class="flex items-center justify-center w-screen h-screen font-medium">
        <div class="flex flex-grow items-center justify-center h-full text-gray-600 bg-gray-100">
            <div class="flex flex-grow items-center justify-center bg-gray-900 h-full">
                <div class="max-w-full p-8 bg-gray-800 rounded-lg shadow-lg w-fit h-3/4 overflow-y-auto text-gray-200">
                    <div class="flex items-center mb-3">
                        <svg class="h-8 w-8 text-indigo-500 stroke-current" xmlns="http://www.w3.org/2000/svg" fill="none"
                            viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M20 13V6a2 2 0 00-2-2H6a2 2 0 00-2 2v7m16 0v5a2 2 0 01-2 2H6a2 2 0 01-2-2v-5m16 0h-2.586a1 1 0 00-.707.293l-2.414 2.414a1 1 0 01-.707.293h-3.172a1 1 0 01-.707-.293l-2.414-2.414A1 1 0 006.586 13H4" />
                        </svg>
                        <h4 class="font-semibold ml-3 text-lg">Sam's Jobs</h4>
                    </div>
                    <button class="flex items-center w-full h-8 px-2 mt-2 text-sm font-medium rounded mb-2">
                        <svg class="w-5 h-5 text-gray-400 fill-current" xmlns="http://www.w3.org/2000/svg" fill="none"
                            viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M12 6v6m0 0v6m0-6h6m-6 0H6" />
                        </svg>
                        <input class="flex-grow h-8 ml-4 bg-transparent focus:outline-none font-medium" type="text"
                            v-model="newTodo" @keydown.enter="addTodo" placeholder="add a new task">
                    </button>
                    <div v-for="todo in todos" :key="todo.id">
                        <TodoItem :todo="todo" @editTodo="editTodo" @deleteTodo="deleteTodo"
                            @toggleComplete="toggleComplete" />
                    </div>

                </div>

            </div>
        </div>

    </div>
</template>
  
<script setup>
import { ref, onMounted } from 'vue';
import TodoItem from '@/components/TodoItem.vue';
import axios from 'axios';

const newTodo = ref('');
const todos = ref([]);

const addTodo = () => {
    if (newTodo.value.trim() === '') return;
    const newTodoItem = {
        userId: 1,
        id: todos.value.length + 1,
        title: newTodo.value,
        completed: false,
    };
    todos.value.unshift(newTodoItem);
    newTodo.value = '';
};

const editTodo = (obj) => {
    todos.value = todos.value.map((item) => {
        if (item.id === obj.todo.id) {
            item.title = obj.newTitle;
        }
        return item;
    });
};

const deleteTodo = (obj) => {
    todos.value = todos.value.filter((item) => item.id !== obj.id);
};

const toggleComplete = (obj) => {
    todos.value = todos.value.map((item) => {
        if (item.id === obj.id) {
            item.completed = !item.completed;
        }
        return item;
    });
};

onMounted(() => {
    axios.get('https://jsonplaceholder.typicode.com/todos').then((response) => {
        todos.value = response.data;
    });
});
</script>
  