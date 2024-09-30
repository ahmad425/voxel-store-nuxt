<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container-fluid">
      <a class="navbar-brand" href="#">
        <b>Voxel</b> <span>Store</span>
      </a>

      <button class="cart" @click="openCheckout = true">
        <img src="../../assets/checkout.png" />
        <div v-if="prodToCart.length">{{ prodToCart.length }}</div>
      </button>

      <Checkout
        :openCheckout="openCheckout"
        :prodToCart="prodToCart"
        @closeCheckout="openCheckout = false"
        @prodAddedOrRemovedFromCart="$emit('prodAddedOrRemovedFromCart', $event)"
      />
    </div>
  </nav>
</template>

<script>
import Checkout from "../products/Checkout.vue";

export default {
  name: "Header",
  components: {
    Checkout,
  },
  props: ["prodToCart"],
  emits: ["prodAddedOrRemovedFromCart"],
  data() {
    return {
      openCheckout: false,
    };
  },
};
</script>

<style lang="scss" scoped>
.brand {
  span {
    color: #1a1a1a80;
  }
}
.cart {
  border: 0;
  height: 21px;
  width: 21px;
  margin-right: 20px;
  display: flex;
  align-items: center;
}
</style>
