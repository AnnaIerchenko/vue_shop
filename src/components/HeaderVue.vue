<script setup>
defineProps({
  totalPrice: Number
})
  const emit = defineEmits(['openDrawer'])

</script>
<template>
    <header class="flex justify-between border-b border-slate-300 px-14 py-8">
      <router-link to="/">
      <div class="flex items-center gap-4">
        <img src="/logo.png" alt="logo" class="w-10">
      <div>
        <h2 class="text-xl font-bold uppercase">Vue commerce</h2>
        <p class="text-slate-400">Commerce the best shoes</p>
      </div>
    </div>
    </router-link>

    <ul class="flex items-center gap-10">
      <li class="flex items-center gap-3 text-slate-500 hover:text-black cursor-pointer" @click="() => emit('openDrawer')">
        <img src="/cart.svg" alt="cart">
        <b>{{totalPrice}}$</b>
      </li>
    <router-link to="/favorites">
      <li class="flex items-center gap-3 text-slate-400 hover:text-black cursor-pointer">
        <img src="/heart.svg" alt="heart">
        <span>Favourites</span>
      </li>
    </router-link>
      <li class="flex items-center gap-3 text-slate-400 hover:text-black cursor-pointer">
        <img src="/profile.svg" alt="photo">
        <span>Profile</span>
      </li>
    </ul>
    </header>


</template>