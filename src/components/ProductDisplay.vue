<template>
  <!-- Loader -->
  <div v-if="loading" class="loader-container">
    <div class="loading-spinner"></div>
  </div>

  <!-- Product Content -->
  <div v-else class="container">
    <!-- Men's Clothing -->
    <section v-if="isMensClothing" class="men-section">
      <div class="bg-container">
        <img class="bg-img" src="../assets/men-blue-background.jpg" alt="men background" />
      </div>

      <div class="product">
        <div class="product-picture">
          <img :src="productData.image" alt="product" class="product-preview" />
        </div>

        <div class="product-detail">
          <h2 class="product-name-men">{{ productData.title }}</h2>

          <div class="category-rating">
            <p class="category">{{ productData.category }}</p>
            <div class="rating">
              <p class="rating-number">{{ productData.rating.rate }}/5</p>
              <div class="circles-men" :style="{ '--rating': productData.rating.rate }"></div>
            </div>
          </div>

          <div class="description-container">
            <p class="description">{{ productData.description }}</p>
          </div>

          <p class="price-men">{{ "$" + productData.price }}</p>

          <div class="action-button">
            <button class="buy-button-men">Buy Now</button>
            <button class="next-button-men" @click="getNextProduct">Next Product</button>
          </div>
        </div>
      </div>
    </section>

    <!-- Women's Clothing -->
    <section v-else-if="isWomensClothing" class="women-section">
      <div class="bg-container">
        <img class="bg-img" src="../assets/women-pink-background.jpg" alt="women background" />
      </div>

      <div class="product">
        <div class="product-picture">
          <img :src="productData.image" alt="product" class="product-preview" />
        </div>

        <div class="product-detail">
          <h2 class="product-name-women">{{ productData.title }}</h2>

          <div class="category-rating">
            <p class="category">{{ productData.category }}</p>
            <div class="rating">
              <p class="rating-number">{{ productData.rating.rate }}/5</p>
              <div class="circles-women" :style="{ '--rating': productData.rating.rate }"></div>
            </div>
          </div>

          <div class="description-container">
            <p class="description">{{ productData.description }}</p>
          </div>

          <p class="price-women">{{ "$" + productData.price }}</p>

          <div class="action-button">
            <button class="buy-button-women">Buy Now</button>
            <button class="next-button-women" @click="getNextProduct">Next Product</button>
          </div>
        </div>
      </div>
    </section>

    <!-- Unavailable Product -->
    <section v-else class="unavailable-section">
      <div class="bg-container">
        <img class="bg-img" src="../assets/gray-background.jpg" alt="unavailable background" />
      </div>

      <div class="unavailable-page">
        <p class="unavailable-message">This product is unavailable to show</p>
        <button class="next-button1" @click="getNextProduct">Next Product</button>
      </div>
    </section>
  </div>
</template>

<script>
let productIndex = 1;

export default {
  name: "ProductDisplay",

  data() {
    return {
      loading: false,
      productData: null,
    };
  },

  computed: {
    isMensClothing() {
      return this.productData?.category === "men's clothing";
    },
    isWomensClothing() {
      return this.productData?.category === "women's clothing";
    },
    isProductAvailable() {
      return this.isMensClothing || this.isWomensClothing;
    },
  },

  mounted() {
    this.loadProduct();
  },

  methods: {
    async loadProduct() {
      this.loading = true;
      try {
        const response = await fetch(`https://fakestoreapi.com/products/${productIndex}`);
        this.productData = await response.json();
      } catch (err) {
        console.error("Error fetching product:", err);
      } finally {
        this.loading = false;
      }
    },

    getNextProduct() {
      productIndex = (productIndex % 20) + 1;
      this.loadProduct();
    },
  },
};
</script>

<style>
@import url("../assets/style/pages.css");
</style>