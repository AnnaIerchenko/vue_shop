<script setup>
  import CardVue from './CardVue.vue';

  const onClickAdd = () => {
    alert("Addddd")
  }
</script>

<template>
  <div class="flex justify-center flex-wrap">
    <card-vue 
      imageUrl="/sneakers/sneakers-1.jpg" 
      title="Sneakers Nike Blazer Mid Suede for men"
      :price="125"
      :isFavourite="true"
      :onClickAdd="onClickAdd" 
      :isAdded="true"
    />
  </div>
</template>