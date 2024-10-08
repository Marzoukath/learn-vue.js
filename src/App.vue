<template>
    <form action ="" @submit.prevent="addTodo">

        <fieldset role ="group">
            <input 
                v-model="newTodo"
                type ="text" 
                placeholder="Tâches à effectuer">
            <button :disabled="newTodo == 0">Ajouter</button>
        </fieldset>
    </form>
    <div v-if="todos.length ==0">Vous n'avez pas de tâches à faire :( </div>
    <div v-else=>
        <ul>
            <li 
            v-for="todo in sortedTodos()"
            :key="todo.date"
            :class= "{completed: todo.completed}" >
            <label>
                <input type="checkbox" v-model="todo.completed"> 
                {{ todo.title }}
            </label>
          
            </li> 
        </ul>
        <label>
            <input type="checkbox" v-model="hideCompleted">
            Masquer les taches completees
        </label>
    </div>

</template>

<script setup>

import { ref } from 'vue';

const todos = ref([{
    title: 'Tache de test',
    completed: true,
    date:1,
}])
const newTodo = ref('')
const hideCompleted = ref(false)
const addTodo=() => {
    todos.value.push({
        title: newTodo.value,
        completed: false,
        date: Date.now,

    })
    newTodo.value =''
}    
const sortedTodos = () => {
    const sortedTodos = todos.value.toSorted((a, b) => a.completed > b.completed ? 1: -1)
    if(hideCompleted.value == true){
        return sortedTodos.filter(t =>  t.completed == false)
    }
    return sortedTodos
}



</script>


<style>
.completed {
    opacity: .5;
    text-decoration: line-through;
}
</style>