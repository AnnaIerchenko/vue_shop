<script setup>
  import CardList from '../components/CardList.vue';
  import axios from 'axios';
import { inject, onMounted, reactive, ref, watch } from 'vue';
import debounce from 'lodash.debounce';

  const { cart, addToCart, removeFromCart } = inject('cart');

  const items = ref([])
  const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
})
  const onClickAddPlus = (item) => {
  if(!item.isAdded){
    addToCart(item)
  }else {
    removeFromCart(item)
  }
  console.log(cart)
}
const onChangeSelect = (e) => {
  filters.sortBy = e.target.value
}
const onChangeSearchInput = debounce((e) => {
  filters.searchQuery = e.target.value
}, 300)

const addToFavorite = async (item) => {
  try {
    if(!item.isFavorite){
      const obj = {
        parentId: item.id,
        item
      }
      item.isFavorite = true
      const {data} = await axios.post('https://11d67ba88938e517.mokky.dev/favorites', obj)
      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`https://11d67ba88938e517.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch(err){
    console.log(err)
  }
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
      favoriteId: null,
      isAdded: false
    }))
    // console.log(data)
  }catch(arr){
    console.log(arr)
  }
}
onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : []
  await fetchItems()
  await fetchFavorites()


  //show item checked, if they are in the cart, 
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }))
})
watch(filters, fetchItems)
watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})
</script>

<template>
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
  <card-list :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus"/>
</template>