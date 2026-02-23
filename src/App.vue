<template>

  <Layout>
    <template #header>
      En tete
    </template>
  
    <template #aside>
      Sidebar
    </template>
  
    <template #main>
      Main
    </template>
  
    <template #footer>
      Footer
    </template>
  </Layout>

  <form action="" @submit.prevent="addTodo">
    <fieldset role="group">
      <input
        v-model="newTodo"
        type="text"
        placeholder="Tache a effectuer">
      <button :disabled="newTodo.length === 0">Ajouter</button>
    </fieldset>
  </form>
  
  <div v-if="todos.length === 0">Vous n'avez pas de tache a faire</div>

  <div v-else>
    <ul>
      <li v-for="todo in sortedTodos"
        :key="todo.date"
        :class="{completed: todo.completed}"
        >
          <Checkbox :label="todo.title" @check=""
          v-model="todo.completed" />
      </li>
    </ul>
  </div>

  <div>
    <label>
      <input type="checkbox" v-model="hideCompleted">
      Masquer les taches complétées
    </label>

    <p v-if="remainingTodos > 0">
      {{ remainingTodos }} tache{{ remainingTodos > 1 ? 's' : '' }} à faire
    </p>

    <Checkbox :label="'Bonjour'"/>
  </div>

  
</template>


<script setup>

import { ref, computed } from 'vue';
import Checkbox from './Checkbox.vue';
import Button from './Button.vue';
import Layout from './Layout.vue';

const newTodo = ref('')

const hideCompleted = ref(false)

const todos = ref ([{
  title: 'Tache de test',
  completed: true,
  date: 1
}, {
  title: 'Tache a faire',
  completed: false,
  date: 2
}])

const addTodo = () => {
  todos.value.push({
    title: newTodo.value,
    completed: false,
    date: Date.now()
  })
  newTodo.value = ''
}

const sortedTodos = computed(() => {
  const sortedTodos = todos.value.toSorted((a, b) => a.completed > b.completed ? 1 : -1)
  if (hideCompleted.value === true) {
    return sortedTodos.filter(t => t.completed === false)
  }
  return sortedTodos
})

const remainingTodos = computed(() => {
  return todos.value.filter(t => t.completed === false).length
})


</script>


// CSS
<style>


.completed {
  opacity: .5;
  text-decoration: line-through;
}

</style>