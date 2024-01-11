<script setup>
import CardVue from './CardVue.vue';

  defineProps({
    items: Array,
  })
  //function props
  const emit = defineEmits(['addToFavorite', 'addToCart'])
 
</script>

<template>
  <div class="grid grid-cols-4 gap-5" v-auto-animate>
    <card-vue 
      v-for="item in items"
      :id="item.id"
      :key="item.id"
      :imageUrl="item.imageUrl" 
      :title="item.title"
      :price="item.price"
      :onClickFavorite="() => emit('addToFavorite', item)"
      :onClickAdd="() => emit('addToCart', item)"
      :isFavorite="item.isFavorite"
      :isAdded="item.isAdded"
    />
  </div>
</template>