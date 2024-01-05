<script setup>
import { onMounted, reactive, ref, watch } from 'vue';
import HeaderVue from './components/HeaderVue.vue';
import CardList from './components/CardList.vue';
import axios from 'axios';
// import DrawerView from './components/DrawerView.vue';

// onMounted(() => {
  // fetch('https://11d67ba88938e517.mokky.dev/items')
  //   .then((res) => res.json())
  //   .then((data) => {
  //     console.log(data)
  //   })
  // axios.get('https://11d67ba88938e517.mokky.dev/items').then((resp) => console.log(resp.data))
// })
const items = ref([])
// { value: []}

const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
})
const onChangeSelect = (e) => {
  filters.sortBy = e.target.value
}
const onChangeSearchInput = (e) => {
  filters.searchQuery = e.target.value
}
const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://11d67ba88938e517.mokky.dev/favorites`)
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)
      if(!favorite) {
        return item
      }
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id,
      }
    })
    console.log(items.value)
  }catch(err){
    console.log(err)
  }
}

const fetchItems = async() => {
  try {
    const params = {
      sortBy: filters.sortBy,
    }
    if(filters.searchQuery){
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get(`https://11d67ba88938e517.mokky.dev/items`, {
      params
  })
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      isAdded: false
    }))
    // console.log(data)
  }catch(arr){
    console.log(arr)
  }
}
onMounted(async () => {
  fetchItems()
  fetchFavorites()
})
watch(filters, fetchItems)
</script>

<template>
  <!-- <drawer-view /> -->
    <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
      <header-vue />
      <div class="p-10">
        <div class="flex justify-between items-center">
          <h2 class="text-3xl font-bold mb-8">All SNEAKERS</h2>
          <div class="flex gap-4">
            <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
            <option value="name">By name</option>
            <option value="price">By price (cheep)</option>
            <option value="-price">By price (expensive)</option>
          </select>
          </div>
        

          <div class="relative">
            <img class="absolute left-4 top-3" src="/search.svg" alt="search" />
            <input 
              @input="onChangeSearchInput"
              type="text"
              class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
              placeholder="Search..."
            />
          </div>
        </div>
        <card-list :items="items" />
      </div>
    </div>
</template>

<style scoped>

</style>
