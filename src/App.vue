<script setup lang="ts">
import { Divider, InputText } from 'primevue';
import Card from 'primevue/card';
import { ref } from 'vue';
  const goals = ref('');
  const goalsList = ref<string[]>([]);
  function addGoal() {
    if (goals.value.trim() !== '') {
      goalsList.value.push(goals.value);
      goals.value = '';
    }
  }
</script>

<template>
  <form @submit.prevent="addGoal" class="flex  flex-col bg-teal-500 justify-start items-center h-screen">

    <h1 class=" text-7xl text-white  underline  ">Hello Vue! </h1>
    <Divider align="center" class="  bg-transparent mt-5 " ><b class="bg-transparent ">vue</b></Divider>
    <span class="text-4xl text-center font-bold capitalize mt-9 flex ">set goals</span>
    <InputText class="w-1/2 mt-5 bg-transparent border-0 shadow-2xl placeholder:text-white"
      placeholder="Enter your goal" v-model="goals" />
      <button class="mt-5 bg-teal-400 text-white px-4 py-2 rounded" @click="addGoal">Add Goal</button>
      <Card class="w-1/2 mt-5 bg-transparent border-teal-400 border-2 outline-0 shadow-2xl text-white">
   <template #content>
    <div>

      <p class=" capitalize text-shadow-taupe-700 text-center text-5xl">TaskList</p>
      <ul class="list-disc pl-5">
        <li v-for="goal in goalsList" :key="goal" class="text-white text-2xl capitalize">{{ goal }}</li>
      </ul>
    </div>
  </template>
      </Card>
  </form>

</template>
