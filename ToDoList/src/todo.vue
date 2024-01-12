<script setup>
import { ref, onMounted, computed, watch } from 'vue';
const dailydata=ref(null)
const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref('')

const todo_asc = computed(() => todos.value.sort((a, b) => {
  return b.createdAt - a.createdAt
}))

const addTodo = () => {
  if (input_content.value.trim()==='' || input_category.value===null){
    return;
  }

  const newTodo = {
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime(),
    completedAt: null
  };

  todos.value.push(newTodo)

  input_content.value=''
  input_category.value=null
}

const Complete = todo => {
  const index = todos.value.findIndex((t) => t === todo);
  if(todos.value[index].completedAt==null){
    todos.value[index].completedAt = new Date().getTime();
  }else{
    todos.value[index].completedAt = null;
  }
  
}

const removeTodo = todo => {
  todos.value = todos.value.filter(t => t!= todo)
}

watch(todos, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, {deep: true})

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos'))||[]
})
</script>

    
        
<template>
    
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        What's Up, <input type="text" placeholder="Name here" v-model="name" />
      </h2>
    </section>

    <section class="create-todo">
      <h3>CREATE A TODO</h3>

      <form @submit.prevent="addTodo">
          <h4>Whats on your todo List?</h4>
          <input type="text" placeholder="e.g Study DataStructures" v-model="input_content" required/>

          <h4>Pick a Category</h4>
          <div class="options" aria-required="true">

              <label>
                <input type="radio" name="category" value="business" v-model="input_category" required/>
                <span class="bubble business"></span>
                <div>Business</div>
              </label>
              
              <label>
                <input type="radio" name="category" value="personal" v-model="input_category" required/>
                <span class="bubble personal"></span>
                <div>Personal</div>
              </label>

          </div>
          <input type="submit" value="Add todo"/>
          <!-- <span v-if="!input_category" class="error-message"><h4>Please select a category</h4></span> -->
      </form>
    </section>

    <section class="todo-list">
      <h3>TODO LIST</h3>
      <div class="list">
        <!-- <div v-for="todo in todo_asc" :class="'todo-item ${todo.done && 'done'}'"> -->
          <div v-for="todo in todo_asc" :class="'todo-item ' + (todo.done ? 'done' : '')">
          <label>
            <input type="checkbox" v-model="todo.done" @click="Complete(todo)"/>
            <span :class="'bubble ${todo.category}'"></span>
          </label>

          <div class="todo-content">
            <input type="text" v-model="todo.content"/>
          </div>

          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Delete</button>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>
