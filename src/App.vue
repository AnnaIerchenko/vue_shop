<script setup>
import { computed, provide, ref, watch } from 'vue';
import HeaderVue from './components/HeaderVue.vue';
import DrawerView from './components/DrawerView.vue';
// import HomePage from './pages/HomePage.vue';

// onMounted(() => {
  // fetch('https://11d67ba88938e517.mokky.dev/items')
  //   .then((res) => res.json())
  //   .then((data) => {
  //     console.log(data)
  //   })
  // axios.get('https://11d67ba88938e517.mokky.dev/items').then((resp) => console.log(resp.data))
// })

// { value: []}
const cart = ref([])
const drawerOpen = ref(false)

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))
const taxPrice = computed(() => Math.round((totalPrice.value * 5) / 100))


const closeDrawer = () => {
  drawerOpen.value = false
}
const openDrawer = () => {
  drawerOpen.value = true
}


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
  />
    <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
      <header-vue :total-price="totalPrice" @open-drawer="openDrawer"/>
      <div class="p-10">
        <router-view></router-view>
      </div>
    </div>
</template>

<style scoped>

</style>
