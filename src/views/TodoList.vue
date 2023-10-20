<template>
    <div class="flex items-center justify-center w-screen h-screen font-medium">
        <div class="flex flex-grow items-center justify-center h-full text-gray-600 bg-gray-100">
            <div class="flex flex-grow items-center justify-center bg-gray-900 h-full">
                <div class="max-w-full p-8 bg-gray-800 rounded-lg shadow-lg w-fit h-3/4 overflow-y-auto text-gray-200">
                    <div class="flex items-center mb-3">
                        <IconCard class="w-8 h-8" />
                        <h4 class="font-semibold ml-3 text-lg">Todo List</h4>
                    </div>
                    <div class="flex items-center w-full h-8 px-2 mt-2 text-sm font-medium rounded mb-2">
                        <IconPlus class="w-6 h-6 cursor-pointer" @click="addTodo" />
                        <input class="flex-grow h-8 ml-4 bg-transparent focus:outline-none font-medium" type="text"
                            id="newTodo" v-model="newTodo" @keydown.enter="addTodo" placeholder="add a new task" />
                    </div>
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
import { ref, onBeforeMount } from 'vue'
import TodoItem from '@/components/TodoItem.vue'
import IconCard from '@/components/icons/IconCard.vue'
import IconPlus from '@/components/icons/IconPlus.vue'
import axios from 'axios'

const newTodo = ref('');
const todos = ref([]);

const addTodo = () => {
    if (newTodo.value.trim() === '') return;
    const newTodoItem = {
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

onBeforeMount(() => {
    axios.get('https://jsonplaceholder.typicode.com/todos').then((response) => {
        todos.value = response.data;
    });
});
</script>
