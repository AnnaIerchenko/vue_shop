<script setup>
import HeaderVue from './components/HeaderVue.vue';
import CardList from './components/CardList.vue';
import DrawerView from './components/DrawerView.vue';

</script>

<template>
  <drawer-view />
    <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
      <header-vue />
      <div class="p-10">
      <h2 class="text-3xl font-bold mb-8">All SNEAKERS</h2>
        <card-list />
      </div>
    </div>
</template>

<style scoped>

</style>
