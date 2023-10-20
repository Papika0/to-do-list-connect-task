<template>
  <div class="flex flex-row justify-between">
    <input class="hidden" type="checkbox" :id="todo.id" @change="toggleComplete" />
    <label class="flex items-center h-10 px-2 rounded cursor-pointer hover:bg-gray-900" :for="todo.id">
      <span :class="{ 'bg-green-500 border-green-500': todo.completed }"
        class="flex items-center justify-center w-5 h-5 text-transparent border-2 border-gray-500 rounded-full">
        <svg class="w-4 h-4 fill-current" :class="{ 'text-white': todo.completed }" xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 20 20" fill="currentColor">
          <path fill-rule="evenodd"
            d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"
            clip-rule="evenodd" />
        </svg>
      </span>
      <span class="ml-4 text-sm" @dblclick="
        editMode = true;
      $nextTick(() => $refs.editInput.focus())
        ">
        <input :class="{ 'line-through': todo.completed }" class="bg-transparent outline-none" ref="editInput"
          :id="todo.id + 'id'" v-model="editedTitle" @keydown.enter="saveEdit" @keydown.esc="cancelEdit"
          :disabled="!editMode" :style="{ width: `${editedTitle.length}ch` }" />
      </span>
    </label>

    <div class="inline-flex gap-5">
      <button @click="
        editMode = true;
      $nextTick(() => $refs.editInput.focus())
        " v-if="!editMode">
        Edit
      </button>
      <IconDeleteX class="w-4 h-4 my-auto cursor-pointer" @click="deleteItem">Delete</IconDeleteX>
    </div>
  </div>
</template>

<script setup>
import { ref, defineEmits } from 'vue'
import IconDeleteX from '@/components/icons/IconDeleteX.vue'

const emit = defineEmits()

const props = defineProps({
  todo: Object
})

const editMode = ref(false)
const editedTitle = ref(props.todo.title)

const saveEdit = () => {
  editMode.value = false
  emit('editTodo', { todo: props.todo, newTitle: editedTitle.value })
}

const cancelEdit = () => {
  editMode.value = false
  editedTitle.value = props.todo.title
}

const deleteItem = () => {
  emit('deleteTodo', props.todo)
}

const toggleComplete = () => {
  emit('toggleComplete', props.todo)
}
</script>
