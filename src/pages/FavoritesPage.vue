<script setup>
  import { onMounted, ref } from "vue";
  import axios from "axios";
  import CardList from '../components/CardList.vue';

  const favorites = ref([])

  onMounted(async() => {
    try {
      const { data } = await axios.get(`https://11d67ba88938e517.mokky.dev/favorites/${favorites.value}`)
      // favorites.value = data.map((obj) => obj.item)
      // console.log(data.map(item => item))
      favorites.value = data.map(obj => obj.item)
    } catch(err) {
      console.log(err)
    }
  })
</script>

<template>
  <div>
    <h2 class="text-3xl font-bold mb-8">My Favorites</h2>
    <card-list :items="favorites" is-favorites/>
  </div>
</template>


