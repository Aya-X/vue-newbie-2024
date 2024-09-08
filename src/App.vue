<template>
  <div class="min-h-screen py-12 bg-gradient-to-br from-amber-100 via-amber-200 to-amber-300">
    <div class="container mx-auto px-4">
      <h1 class="text-4xl font-bold mb-10 text-center text-amber-800">ONLINE TODO LIST</h1>

      <div
        class="bg-white bg-opacity-90 rounded-2xl shadow-2xl p-8 max-w-md mx-auto backdrop-filter backdrop-blur-sm h-[calc(50vh+4rem)] flex flex-col">
        <!-- Todo Input -->
        <div class="mb-6 flex">
          <input v-model="newTodo" @keyup.enter="handleAddTodo" type="text" placeholder="請輸入待辦事項"
            class="flex-grow p-3 border-2 border-amber-300 rounded-l-lg focus:outline-none focus:ring-2 focus:ring-amber-400 focus:border-transparent transition duration-300">
          <button @click="handleAddTodo"
            class="w-12 bg-gradient-to-r from-amber-400 to-amber-500 text-white p-3 rounded-r-lg hover:from-amber-500 hover:to-amber-600 focus:outline-none focus:ring-2 focus:ring-amber-400 transition duration-300">
            +
          </button>
        </div>

        <!-- Todo Tabs -->
        <ul class="flex justify-between mb-6 bg-amber-100 rounded-lg p-1">
          <li v-for="tab in tabs" :key="tab" class="flex-1">
            <a href="#" @click.prevent="currentTab = tab" :class="[
              'px-4 py-2 text-center font-semibold rounded-md transition duration-300 block',
              currentTab === tab
                ? 'bg-white text-amber-700 shadow-md'
                : 'text-amber-700 hover:bg-white hover:text-amber-600'
            ]">
              {{ tab }}
            </a>
          </li>
        </ul>

        <!-- Todo List -->
        <div class="flex-grow overflow-y-auto">
          <div v-if="filteredTodos.length === 0" class="text-center text-amber-700 my-8 italic">
            目前尚無待辦事項
          </div>
          <ul v-else class="space-y-4">
            <li v-for="todo in filteredTodos" :key="todo.id" :class="[
              'flex items-center p-4 bg-white rounded-lg shadow-md transition duration-300 hover:shadow-lg border-l-4',
              todo.completed ? 'border-amber-300' : 'border-amber-500'
            ]">
              <input type="checkbox" :checked="todo.completed" @change="handleToggleTodo(todo)"
                class="mr-4 w-5 h-5 text-amber-500 focus:ring-amber-400 rounded transition duration-300">
              <span :class="[
                'flex-grow transition duration-300',
                todo.completed ? 'line-through text-gray-400' : 'text-gray-800'
              ]">
                {{ todo.text }}
              </span>
              <button @click="handleRemoveTodo(todo)"
                class="ml-2 text-amber-600 hover:text-amber-700 transition duration-300">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd"
                    d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
                    clip-rule="evenodd" />
                </svg>
              </button>
            </li>
          </ul>
        </div>

        <!-- Todo Status -->
        <div class="mt-6 text-sm text-amber-700 font-semibold">
          {{ remainingTodos }} 個待完成項目
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const todos = ref([])
const newTodo = ref('')
const currentTab = ref('全部')
const tabs = ['全部', '待完成', '已完成']

const filteredTodos = computed(() => {
  if (currentTab.value === '全部') {
    return todos.value
  }
  return todos.value.filter(todo =>
    currentTab.value === '已完成' ? todo.completed : !todo.completed
  )
})

const remainingTodos = computed(() =>
  todos.value.filter(todo => !todo.completed).length
)

const handleAddTodo = () => {
  todos.value = [
    ...todos.value,
    {
      id: Date.now(),
      text: newTodo.value.trim(),
      completed: false
    }
  ]
  newTodo.value = ''
}

const handleRemoveTodo = (todoToRemove) => {
  todos.value = todos.value.filter(todo => todo.id !== todoToRemove.id)
}

const handleToggleTodo = (todoToToggle) => {
  todos.value = todos.value.map(todo =>
    todo.id === todoToToggle.id
      ? { ...todo, completed: !todo.completed }
      : todo
  )
}
</script>