<template>
  <div v-if="openCheckout" class="cart-container">
    <div class="cart-header">
      <h3>Your Cart</h3>
      <img src="../../assets/close.png" @click="$emit('closeCheckout')" />
    </div>
    <div class="cart-products">
      <div class="prod-detail" v-for="(product, i) in cartProducts" :key="i">
        <img :src="product.image" />
        <div class="prod-info">
          <div class="title">{{ product.title }}</div>
          <div class="price-remove">
            <div class="price">
              {{ `$${product.price}` }}
            </div>
            <button class="remove-btn" @click="removeItem(product.id)">Remove</button>
          </div>
        </div>
      </div>
    </div>
    <div class="cart-total">
      <h3>Total</h3>
      <h3>{{ `$${prodTotal}`  }}</h3>
    </div>
    <div class="checkout-btn">
      <button>Checkout</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Checkout',
  props: ['openCheckout', 'prodToCart'],
  emits: ['closeCheckout', 'prodAddedOrRemovedFromCart'],

  computed: {
    cartProducts() {
      const prods = JSON.parse(localStorage.getItem('products')) || [];
      console.log('prods: ', prods);
      console.log('opencheckout:', this.openCheckout)
      console.log('this.prodToCart11: ', this.prodToCart)
      return prods.filter((p) => this.prodToCart.includes(p.id));
    },
    prodTotal() {
      return this.cartProducts.map((p) => p.price).reduce((a, b) => a + b, 0);
    }
  },
  methods: {
    removeItem(id) {
      const prods = this.cartProducts.filter((p) => p.id != id)
      localStorage.setItem('products', JSON.stringify(prods));
      this.$emit('prodAddedOrRemovedFromCart', id);
    }
  }
}
</script>

<style lang="scss" scoped>
.cart-container {
  position: fixed;
  top: 0;
  right: 0;
  border: 3px solid #f1f1f1;
  z-index: 9;
  height: 100%;
  max-width: 450px;
  padding: 20px;
  padding-top: 0;
  background-color: white;

  .cart-header {
    display: flex;
    justify-content: space-between;
    padding-block: 10px;
    padding-top: 20px;
    border-bottom: 1px solid #1A1A1A1A;
    margin-bottom: 20px;

    img {
      height: 20px;
      width: 20px;
      cursor: pointer;
    }
  }

  .prod-detail {
    display: flex;
    padding-block: 10px;

    img {
      width: 80px;
      height: 80px;
      border: 1px solid #1A1A1A1A;
      padding: 7px;
      border-radius: 10px;
      margin-right: 15px;
    }

    .prod-info {
      width: 100%;

      .title {
        color: #1A1A1A;
        font-size: 14px;
        font-weight: 600;
      }
      .price-remove {
        display: flex;
        justify-content: space-between;

        .price {
          color: #1A1A1A80;
          font-size: 16px;
          font-weight: 500;
        }
        button {
          border: 0;
          background: transparent;
          color: #1A1A1A80;
        }
      }
    }
  }

  .cart-total {
    display: flex;
    justify-content: space-between;
    padding-block: 15px;
    margin-top: 20px;
    border-top: 1px solid #1A1A1A1A;
  }

  .checkout-btn {
    button {
      border: 0;
      background-color: #462DDF;
      width: 100%;
      padding-block: 14px;
      display: flex;
      border-radius: 8px;
      align-items: center;
      justify-content: center;
      color: #fff;
    }
  }
}
</style>