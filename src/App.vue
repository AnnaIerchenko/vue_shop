<script setup>

</script>

<template>
  <div class="bg-white h-screen w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <header class="flex justify-between border-b border-slate-300 px-14 py-8">
      <div class="flex items-center gap-4">
        <img src="/logo.png" alt="logo" class="w-10">
      <div>
        <h2 class="text-xl font-bold uppercase">Vue commerce</h2>
        <p class="text-slate-400">Commerce the best shoes</p>
      </div>
    </div>
    <ul class="flex items-center gap-10">
      <li class="flex items-center gap-3 text-slate-500 hover:text-black cursor-pointer">
        <img src="/cart.svg" alt="cart">
        <b>128$</b>
      </li>
      <li class="flex items-center gap-3 text-slate-400 hover:text-black cursor-pointer">
        <img src="/heart.svg" alt="heart">
        <span>Favourites</span>
      </li>
      <li class="flex items-center gap-3 text-slate-400 hover:text-black cursor-pointer">
        <img src="/profile.svg" alt="photo">
        <span>Profile</span>
      </li>
    </ul>
    </header>
    <h1>About Page</h1>
  </div>
</template>

<style scoped>

</style>
