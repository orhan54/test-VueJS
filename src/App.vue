<template>
  <form action="" @submit.prevent="addTodo">
    <fieldset role="group">
    <input 
      v-model="newTodo"
      type="text"
      placeholder="Tache a effectuer">
    <button :disabled="newTodo.length === 0" style="margin-left: 10px;">Ajouter</button>
  </fieldset>

  <div style="margin-left: 5px; 
              margin-top: 20px; 
              color: green;
              font-size: 20px;" 
              v-if="todos.length > 0 && todos.every(todo => todo.completed)">
              <i class="pi pi-check" style="font-size: 1rem">
                Toutes les taches sont terminer
              </i>
  </div>
  </form>
  <div v-if="todos.length === 0">Vous n'avez pas de tache a faire</div>
  <div v-else>
    <ul>
      <li 
        v-for="todo in sortedTodos()"
        :key="todo.date"
        :class="{completed: todo.completed}"
        >
        <label>
          <input type="checkbox" v-model="todo.completed">
          {{ todo.title }}  
        </label>
      </li>
    </ul>
    <label>
      <input type="checkbox" v-model="hideCompleted">
      Masque les taches terminer
    </label>
  </div>
  
</template>


<script setup>
import { ref } from 'vue';

const newTodo = ref('')

const hideCompleted = ref(false)

const todos = ref([{
  title: 'Tache de test',
  completed: true,
  date: 1,
}, {
  title: 'Tache a faire',
  completed: false,
  date: 2,
}])

const addTodo = () => {
  todos.value.push({
    
    title: newTodo.value,
    completed: false,
    date: Date.now()
  })
  newTodo.value = ''
}

const sortedTodos = () => {
  const sortedTodos = todos.value.toSorted((a, b) => a.completed 
  > b.completed ? 1 : -1)
  if(hideCompleted.value === true) {
    return sortedTodos.filter(t => t.completed === false)
  }
  return sortedTodos
}

</script>


// CSS
<style>


.completed {
  opacity: .5;
  text-decoration: line-through;
}

</style>