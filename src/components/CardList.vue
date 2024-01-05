<script setup>
  import CardVue from './CardVue.vue';

  defineProps({
    items: Array
  })
  const onClickAdd = () => {
    alert("Addddd")
  }
</script>

<template>
  <div class="flex justify-center flex-wrap gap-5">
    <card-vue 
      v-for="item in items"
      :key="item.id"
      :imageUrl="item.imageUrl" 
      :title="item.title"
      :price="item.price"
      :isFavorite="isFavorite"
      :onClickAdd="onClickAdd" 
      :isAdded="isAdded"
    />
  </div>
</template>