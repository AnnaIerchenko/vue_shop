<script setup>
import { computed, inject, ref } from 'vue';
  import DrawerHead from './DrawerHead.vue';
  import CartItemList from './CartItemList.vue';
  import InfoBlock from './InfoBlock.vue';
  import axios from 'axios';

  const props = defineProps({
    totalPrice: Number,
    taxPrice: Number,
  })

  const {
    cart
  } = inject('cart')

  const isCreating = ref(false)
  const orderId = ref(null)

  const createOrder = async () => {
  try {
    isCreating.value = true
    const { data } = await axios.post(`https://11d67ba88938e517.mokky.dev/orders`, {
      items: cart.value,
      totalPrice: props.totalPrice.value,
  })
  cart.value = []
  orderId.value = data.id
  // return data
  } catch(err) {
    console.log(err)
  } finally {
    isCreating.value = false
  }
}

const cartIsEmpty = computed(() => cart.value.length === 0)
const buttonDisabled = computed(() => isCreating.value || cartIsEmpty.value)

</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <drawer-head />

    <div v-if="!totalPrice || orderId" class="flex h-full items-center">
      <info-block 
        v-if="!totalPrice && !orderId"
        title="Cart empty" 
        description="Add items" 
        image-url="/package-icon.png" 
      />
      <info-block 
        v-if="orderId"
        title="Your Order Success"
        :description="`Your order #${orderId} will be deliver soon`"
        image-url="/order-success-icon.png"
    />
    </div>

    <div v-else>
      <cart-item-list />

      <div class="flex flex-col gap-4 mt-7">
        <div class="flex gap-2">
          <span>Total:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ totalPrice }}$</b>
        </div>
        <div class="flex gap-2">
          <span>Tax 5%:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ taxPrice }}$</b>
        </div>
        <button
          :disabled="buttonDisabled"
          @click="createOrder"
          class="bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-300 cursor-pointer hover:bg-lime-600 active:bg-lime-700 transition-all">
            Order
        </button>
      </div>
    </div>
   </div>
</template>