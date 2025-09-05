<template>
  <form action="" @submit.prevent="addTodo">
    <fieldset role="group">
      <input 
      v-model="newTodo"
      type="text"
      placeholder="Tâche à effectuer">
      <button :disabled="newTodo.length==0">Ajouter</button>
    </fieldset>
  </form>
 <div v-if="todos.length == 0">Vous n'avez pas de tâches à faire :(</div> 
 <div v-else>
  <ul>
    <li v-for="todo in sortedTodos"
    :key="todo.date"
    :class="{completed: todo.completed}">
    <label>
      <input type="checkbox" v-model="todo.completed">
      {{ todo.title }}
    </label>
    
  </li>
  </ul>
  <div>
    <label>
      <input type="checkbox" v-model="hideCompleted">
      Masquer les tâches complétées
    </label>
  </div>
  <div v-if="remainingTodo > 0">
    {{ remainingTodo }} tâche{{ remainingTodo > 1 ? 's' : '' }} à compléter
  </div>
 </div>
</template>

<script setup>
  import { computed, ref } from 'vue';
  const newTodo = ref('')
  const hideCompleted = ref(false)
  const todos = ref([{
    title: 'Tâche de test',
    completed: true,
    date: 1
  },{
    title: 'Tâche de test',
    completed: false,
    date: 2
  }])
  const addTodo = () => {
    todos.value.push({
      title:newTodo.value,
      completed: false,
      date: Date.now()
    })
    newTodo.value = ''
  }
  const sortedTodos = computed(() => {
    console.log("demo")
    const sortedTodos = todos.value.toSorted((a, b) => a.completed > b.completed ? 1 : -1)
    if (hideCompleted.value == true) {
      return sortedTodos.filter(t => t.completed == false)
    }
    return sortedTodos
  })

  // leçon: computed pour éviter à chaque fois d'appeler la fonction s'il n'y a pas de changement
  const remainingTodo = computed(() => {
    // return sortedTodos.value.length;
    console.log("remaining todo: ", sortedTodos.value.filter(t => t.completed == false).length)
    return sortedTodos.value.filter(t => t.completed == false).length
  })
</script>

<style>
.completed{
  opacity: .5;
  text-decoration: line-through;
}
</style>
