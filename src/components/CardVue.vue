<script setup>

defineProps({
    id: Number,
    title: String,
    imageUrl: String,
    price: Number,
    isFavorite: Boolean,
    isAdded: Boolean,
    onClickAdd: Function,
    onClickFavorite: Function,
  })

</script>
<template>
  <div class="relative border bg-white border-slate-100 rounded-3xl p-8 cursor-pointer hover:scale-95 transition-all hover:shadow-xl">
    
    <img 
      class="absolute top-8 left-8" 
      :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'" 
      alt="like"
      @click="onClickFavorite"
    />
    <img :src="imageUrl" alt="sneakers">
    <p class="mt-2">{{ title }}</p>

    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400">Price:</span>
        <b>{{ price }}$</b>
      </div>

      <img @click="onClickAdd" :src="!isAdded ? '/plus.svg' : './checked.svg'" alt="plus">
    </div>
  </div>
</template>