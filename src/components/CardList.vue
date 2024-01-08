<script setup>
import CardVue from './CardVue.vue';

  defineProps({
    items: Array,
  })
  //function props
  const emit = defineEmits(['addToFavorite'])
  const onClickAdd = () => {
    // alert("Addddd")
  }

 
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <card-vue 
      v-for="item in items"
      :id="item.id"
      :key="item.id"
      :imageUrl="item.imageUrl" 
      :title="item.title"
      :price="item.price"
      :onClickAdd="onClickAdd"
      :onClickFavorite="() => emit('addToFavorite', item)"
      :isFavorite="item.isFavorite"
    />
  </div>
</template>