<template>
  <div class="products-page">
    <div class="products-header">
      <Header
        :prodToCart="prodToCart"
        @prodAddedOrRemovedFromCart="prodRemovedFromCart($event)"
      />
      <Welcome />
    </div>
    <div class="products-categories">
      <div v-if="isMobileDevice()">
        <select v-model="selectedCategory" @change="categoryChanged()">
          <option v-for="(cat, i) in categories" :key="i" :value="cat">
            {{ cat }}
          </option>
        </select>
        <ListProducts
          :filteredProducts="filteredProducts"
          :prodToCart="prodToCart"
          @prodAddedToCart="prodAddedToCart($event)"
        />
      </div>
      <div v-else class="container">
        <nav class="category-tabs">
          <div class="nav nav-tabs" id="nav-tab" role="tablist">
            <button v-for="(cat, i) in categories" :key="`${i}-key`" class="nav-link" :class="`${i == 0 ? 'active' : ''}`" :id="`nav-${i}-tab`" data-bs-toggle="tab" :data-bs-target="`#nav-${i}`" type="button" role="tab" :aria-controls="`nav-${i}`" aria-selected="true" @click="categoryChanged(cat)">{{ cat }}</button>
          </div>
        </nav>
        <div class="tab-content" id="nav-tabContent">
          <div v-for="(cat, i) in categories" :key="`${i}-key`" class="tab-pane fade" :class="`${i == 0 ? 'show active' : ''}`" :id="`nav-${i}`" role="tabpanel" :aria-labelledby="`nav-${i}-tab`" tabindex="0">
            <ListProducts
              :filteredProducts="filteredProducts"
              :prodToCart="prodToCart"
              @prodAddedToCart="prodAddedToCart($event)"
            />
          </div>
        </div>
      </div>
    </div>
    <Footer />
  </div>
</template>

<script>
import ListProducts from "../components/products/ListProducts.vue";
import Header from "../components/layout/Header.vue";
import Welcome from "../components/layout/Welcome.vue";
import Footer from "../components/layout/Footer.vue";

export default {
  components: {
    Header,
    Welcome,
    ListProducts,
    Footer,
  },
  name: "IndexPage",
  data() {
    return {
      products: [],
      filteredProducts: [],
      categories: [],
      prodToCart: [],
      selectedCategory: "All",
    };
  },
  created() {
    this.fetchCategories();
    this.fetchProducts();
  },
  methods: {
    async fetchProducts() {
      try {
        const response = await fetch("https://fakestoreapi.com/products");
        const prods = await response.json();
        this.products = prods;
        this.filteredProducts = prods;
      } catch (error) {
        console.error(error);
      }
    },
    async fetchCategories() {
      try {
        const response = await fetch(
          "https://fakestoreapi.com/products/categories"
        );
        const cats = await response.json();
        cats.unshift("All");
        this.categories = cats;
        console.log(this.categories);
      } catch (error) {
        console.error(error);
      }
    },
    categoryChanged(category = "All") {
      let cat = category;
      if (this.isMobileDevice()) {
        cat = this.selectedCategory;
        console.log("this.selectedCategory", this.selectedCategory);
      }
      if (cat == "All") {
        this.filteredProducts = this.products;
      } else {
        this.filteredProducts = this.products.filter((p) => p.category == cat);
      }
    },
    prodAddedToCart(id) {
      const cartProds = this.prodToCart;
      cartProds.push(id);
      this.prodToCart = cartProds;
      console.log("this.prodToCart:", this.prodToCart);
    },
    prodRemovedFromCart(id) {
      this.prodToCart = this.prodToCart.filter((p) => p != id);
      console.log("this.prodToCart:", this.prodToCart);
    },
    isMobileDevice() {
      const toMatch = [
        /Android/i,
        /webOS/i,
        /iPhone/i,
        /iPad/i,
        /iPod/i,
        /BlackBerry/i,
        /Windows Phone/i,
      ];
      return toMatch.some((toMatchItem) => {
        return navigator.userAgent.match(toMatchItem);
      });
    },
  },
};
</script>

<style lang="scss" scoped>
.products-categories {
  .nav-tabs {
    .nav-link {
      color: #1a1a1a80;
    }
    .nav-item.show {
      .nav-link {
        background-color: #fff;
        border-bottom: 4px solid #1a1a1a;
        color: #1a1a1a;
      }
    }
    .nav-link.active {
      background-color: #fff;
      border: 0;
      border-bottom: 4px solid #1a1a1a;
      color: #1a1a1a;
    }
  }
  .category-tabs {
    margin-bottom: 30px;
  }
  select {
    width: 90%;
    margin-inline: 20px;
    margin-bottom: 30px;
    border-radius: 10px;
    padding: 15px;
    border: 1px solid rgba(26, 26, 26, 0.102);
  }
}
</style>
