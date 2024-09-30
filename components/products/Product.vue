<template>
  <div class="product-container">
    <img :src="product.image" />
    <div class="prod-info">
      <div class="title">{{ product.title }}</div>
      <div class="price">{{ `$${product.price}` }}</div>
    </div>
    <button v-if="isCardAdded(product.id)" class="cart-btn">Added To Cart</button>
    <button v-else class="cart-btn" @click="addProductToCart">+ Add to Cart</button>
  </div>
</template>

<script>
export default {
  name: "Product",
  props: ['product', 'prodToCart'],
  emits: ['prodAddedOrRemovedFromCart'],
  data() {
    return {
      searchQuery: "",
      sortKey: "",
      sortOrders: {},
    };
  },
  computed: {
    product1() {
      return "";
    }
  },
  methods: {
    addProductToCart() {
      const prods = JSON.parse(localStorage.getItem('products')) || [];
      prods.push(this.product);
      localStorage.setItem('products', JSON.stringify(prods));
      this.$emit('prodAddedOrRemovedFromCart', this.product.id);
    },
    isCardAdded(id) {
      return this.prodToCart.includes(id)
    }
  },
};
</script>

<style lang="scss" scoped>
.product-container {
  padding: 15px;
  border: 1px solid #1A1A1A1A;
  border-radius: 12px;
  margin-bottom: 20px;

  img {
    // width: 293px;
    height: 300px;
    margin-bottom: 15px;
    width: 100%;
    object-fit: contain;
  }
  .prod-info {
    height: 80px;

    .title {
      color: #1A1A1A;
      font-size: 14px;
      font-weight: 600;
    }
    .price {
      color: #1A1A1A80;
      font-size: 16px;
      font-weight: 500;
    }
  }
  .cart-btn {
    margin-top: 10px;
    background-color: #F4F4F4;
    display: flex;
    justify-content: center;
    border: 0;
    padding: 10px;
    border-radius: 10px;
    width: 100%;
  }
}
</style>