<script setup>
import { ref, onMounted } from 'vue'
import contacts from "./contacts.json";
const selectedActors = ref([]);
const unselectedActors = ref([]);


function getRandomInt(max) {
  return Math.floor(Math.random() * max);
};

const addRandom = () => {
  const index = getRandomInt(unselectedActors.value.length);
  selectedActors.value.push(unselectedActors.value[index]);
  unselectedActors.value.splice(index, 1);
};

const sortByName = () => {
  selectedActors.value.sort((a, b) => {
    if (a.name > b.name)
      return 1;
    if (a.name === b.name)
      return 0;
    if (a.name < b.name)
      return -1;
  })
};

const sortByPopularity = () => {
  selectedActors.value.sort((a, b) => {
    if (a.popularity > b.popularity)
      return 1;
    if (a.popularity === b.popularity)
      return 0;
    if (a.popularity < b.popularity)
      return -1;
  })
};

const removeActor = (actor) => {
  const index = selectedActors.value.indexOf(actor);
  if (index >= 0) {
    selectedActors.value.splice(index, 1);
    unselectedActors.value.push(actor);
  }
}

onMounted(() => {
  console.log(contacts[0]);
  for (let i = 0; i < 5; i++) {
    selectedActors.value.push(contacts[i]);
  }
  for (let i = 5; i < contacts.length; i++) {
    unselectedActors.value.push(contacts[i]);
  }

})
</script>

<template>
  <h1>Contacts</h1>
  <button @click="addRandom">Add Random</button>
  <button @click="sortByName">Sort by name</button>
  <button @click="sortByPopularity">Sort by popularity</button>
  <table>
    <thead class="header">
      <td>Picture</td>
      <td>Name</td>
      <td>Popularity</td>
      <td>Won an oscar</td>
      <td>Won an Emmy</td>
      <td>Remove</td>
    </thead>
    <tr v-for="actor in selectedActors">
      <td>
        <img :src="actor.pictureUrl" class="actor-picture">
      </td>
      <td>{{ actor.name }}</td>
      <td>{{ actor.popularity }}</td>
      <td>
        <img v-if="actor.wonOscar === true" src="../public/favicon.ico">
      </td>
      <td>
        <img v-if="actor.wonEmmy === true" src="../public/favicon.ico">
      </td>
      <td><button @click=removeActor(actor)>Remove</button></td>
    </tr>
  </table>
</template>

<style>
table {
  margin-top: 30px;
}

td {
  text-align: center;
  border-style: solid;
  border-width: 1px;
  min-width: 150px;
}

.header {
  font-weight: bold;
  font-size: 20px;
}

.actor-picture {
  width: 150px;
}
</style>