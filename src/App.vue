<script setup>
import { computed, onMounted, provide, reactive, ref, watch } from 'vue';
import HeaderVue from './components/HeaderVue.vue';
import CardList from './components/CardList.vue';
import DrawerView from './components/DrawerView.vue';
import axios from 'axios';
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
const cart = ref([])
const isCreatingOrder = ref(false)

const drawerOpen = ref(false)

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))
const taxPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const cartIsEmpty = computed(() => cart.value.length === 0)
const cartButtonDisabled = computed(() => isCreatingOrder.value || cartIsEmpty.value)

const closeDrawer = () => {
  drawerOpen.value = false
}
const openDrawer = () => {
  drawerOpen.value = true
}

const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
})
const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}
const removeFromCart = (item) => {
  const index = cart.value.indexOf(item);
  if (index > -1) {
    cart.value.splice(index, 1);
    item.isAdded = false;
  }
}
const createOrder = async () => {
  try {
    isCreatingOrder.value = true
    const { data } = await axios.post(`https://11d67ba88938e517.mokky.dev/orders`, {
      items: cart.value,
      totalPrice: totalPrice.value,
  })
  cart.value = []
  return data
  } catch(err) {
    console.log(err)
  } finally {
    isCreatingOrder.value = false
  }
}
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
const addToFavorite = async (item) => {
  try {
    if(!item.isFavorite){
      const obj = {
        parentId: item.id
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
watch(cart, 
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  {deep: true}
)

provide('cart', {
  cart,
  closeDrawer,
  openDrawer,
  addToCart,
  removeFromCart,
})

</script>

<template>
  <drawer-view 
    v-if="drawerOpen" 
    :totalPrice="totalPrice" 
    :taxPrice="taxPrice"
    @createOrder="createOrder"
    :button-disabled="cartButtonDisabled"
  />
    <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
      <header-vue :total-price="totalPrice" @open-drawer="openDrawer"/>
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
        <card-list :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus"/>
      </div>
    </div>
</template>

<style scoped>

</style>
