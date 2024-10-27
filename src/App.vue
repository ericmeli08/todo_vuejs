<template>
  <div id="container">
    <header id="header">
      <h1 class="text-3xl font-bold my-10">CREATION D'UNE APPLICATION DE TODO-LIST </h1>
      <div class="flex">
        <input type="text" placeholder="Tache a ajouter" name="text" id="input-text" v-model="newTask">
        <button class="btn-save" @click="saveTask">Enregistrer</button>
      </div>
      <main>

        <h1 class="text-2xl font-800 my-8">TODO-LIST</h1>
        <h2 class="text-xl font-500 my-5" v-if="tasks.length == 0"> Vous navez aucune tache a faire </h2>
        <ul>
          <li v-for="task in sorted" :key="task.date" :class="{ 'opacity-50': task.completed }" class="flex items-center ">
            <span :class="{ 'line-through': task.completed }">{{ task.title }}</span>
            <input type="checkbox" v-model="task.completed" :id="`checkbox-${task.date}`" class="hidden" />
            <label :for="`checkbox-${task.date}`" class="flex items-center cursor-pointer  ">
              <div class="w-8 h-8 border-2  border-gray-300 rounded grid place-items-center "
                :class="{ 'bg-blue-500': task.completed }">
                <span v-if="task.completed" class="text-white font-bold text-lg w-fit h-fit m-0">✓</span>
              </div>
            </label>
            <button @click="deleteTask(task)">supprimer</button>
          </li>
        </ul>
        <div>
          <input type="checkbox" v-model="taskDone" :id="`checkbox`" class="hidden" />
            <label :for="`checkbox`" class="flex items-center cursor-pointer">
              <div class="w-8 h-8 border-2  border-gray-300 rounded grid place-items-center mr-5 "
                :class="{ 'bg-blue-500': taskDone }">
                <span v-if="taskDone" class="text-white font-bold text-lg w-fit h-fit m-0">✓</span>
              </div>
            <span class="text-xl font-bold"> Cachee les taches effectuer</span>
          </label>
        </div>
        <p class="text-xl font-bold" v-if="remain > 0"> {{ remain }} tache{{ remain > 1 ? 's' : '' }} a faire</p>

      </main>
    </header>

  </div>

</template>


<script setup>

import { ref, computed } from 'vue'


const taskDone = ref(false)
let isChecked = ref(false)

let tasks = ref([
  {
    title: 'Se larver',
    completed: true,
    date: 1
  },
  {
    title: 'Manger',
    completed: false,
    date: 2
  }
])

const newTask = ref('')

const deleteTask = (task) => {
  tasks.value = tasks.value.filter(m => m != task)
}

const saveTask = () => {
  tasks.value.push({
    title: newTask.value,
    completed: false,
    date: Date.now()
  })
  newTask.value = ''
}

const toggleCheckbox = () => {
  isChecked = !isChecked;
}

const sorted = computed(() => {
  console.log(tasks.value)
  const sortTodo = tasks.value.toSorted((a, b) => a.completed >= b.completed ? 1 : -1)
  if (taskDone.value) {
    return sortTodo.filter(t => !t.completed)
  }
  return sortTodo
})

const remain = computed(() => {
  return tasks.value.filter(m => !m.completed).length
})

</script>

<style>
#container {
  text-align: center;
  display: grid;
  place-items: center;
}

.barre {
  opacity: 0.5;
}

#container #header {
  width: fit-content;
  margin-top: 50px;
}

#container #header .input {
  width: 100%;
}

#container #header #input-text {
  width: 55%;
  margin-right: 30px;
  border-radius: 5px;
  padding: 10px 15px;
}

#container #header .btn-save {
  background-color: blue;
  padding: 15px 80px;
  border-radius: 5px;
  font-size: 1.1rem;
  font-weight: bold;
  color: white;
}

main {
  margin-top: 40px;
}

main ul {
  margin-top: 40px
}

main ul li {
  margin-bottom: 30px;
}

main li span {
  margin-right: 100px;
  width: 100px;
  font-size: 1.4rem;
  font-weight: bold;
}

main li input {
  font-size: 1.3rem;
}

main button {
  background-color: red;
  padding: 10px 40px;
  border-radius: 5px;
  font-size: 1.1rem;
  font-weight: bold;
  color: white;
  margin-left: 15px;
}
</style>