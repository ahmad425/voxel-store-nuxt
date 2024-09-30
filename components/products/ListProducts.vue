<template>
  <div class="products">
    <div class="row">
      <div class="col-sm-6 col-md-4 col-xs-12" v-for="(product, i) in filteredProducts" :key="i">
        <Product 
          :product="product" 
          :prodToCart="prodToCart" 
          @prodAddedOrRemovedFromCart="$emit('prodAddedToCart', ($event))" 
        />
      </div>
    </div>
  </div>
</template>

<script>
import Product from "./Product.vue";

export default {
  name: 'ListProducts',
  components: {
    Product
  },
  props: ['filteredProducts', 'prodToCart'],
  emits: ['prodAddedToCart']
}
</script>