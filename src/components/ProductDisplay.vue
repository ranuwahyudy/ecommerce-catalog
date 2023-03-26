<template>
  <main
    class="main"
    :class="
      !isProductAvailable || isLoading
        ? 'bg-secondary-gray'
        : isProductAvailable && product.category === 'men\'s clothing'
        ? 'bg-pattern bg-secondary-blue'
        : 'bg-pattern bg-secondary-purple'
    "
  >
    <section class="product">
      <div v-if="isLoading" class="container">
        <div class="card">
          <div class="left-card-loading"></div>
          <div class="right-card-loading">
            <div class="loading-title"></div>
            <div class="loading-category"></div>
            <div class="loading-desc"></div>
            <div class="loading-price"></div>
            <div class="loading-button"></div>
          </div>
        </div>
      </div>
      <div v-else class="container">
        <div v-if="!isProductAvailable" class="card">
          <div class="unavailable-product-img">
            <img src="../assets/img/sad-face.svg" alt="Unavailable Product" />
            <div class="unavailable-product-text">
              <p>This product is unavailable to show</p>
              <button @click="getNextProduct" class="btn-unavailable-next">
                Next product
              </button>
            </div>
          </div>
        </div>
        <div v-else class="card">
          <div class="left-card">
            <img :src="product.image" :alt="product.title" />
          </div>
          <div class="right-card">
            <div class="right-card-top">
              <h1
                :class="
                  product.category === 'men\'s clothing'
                    ? 'color-primary-blue'
                    : 'color-primary-purple'
                "
              >
                {{ product.title }}
              </h1>
              <div class="product-category-rating">
                <div class="product-category">
                  <p>{{ product.category }}</p>
                </div>
                <div v-if="product.rating" class="product-rating">
                  <p>
                    {{ product.rating.rate }}/5
                    <span
                      v-for="n in 5"
                      :key="n"
                      :class="
                        product.category === 'men\'s clothing'
                          ? 'color-primary-blue'
                          : 'color-primary-purple'
                      "
                    >
                      <i :class="getRatingClass(n)"></i>
                    </span>
                  </p>
                </div>
              </div>
              <p class="product-desc">{{ product.description }}</p>
            </div>
            <div class="right-card-bottom">
              <h2
                :class="
                  product.category === 'men\'s clothing'
                    ? 'color-primary-blue'
                    : 'color-primary-purple'
                "
              >
                ${{ product.price }}
              </h2>
              <button
                class="btn-buy"
                :class="
                  product.category === 'men\'s clothing'
                    ? 'bg-primary-blue border-primary-blue'
                    : 'bg-primary-purple border-primary-purple'
                "
              >
                Buy now
              </button>
              <button
                @click="getNextProduct"
                class="btn-next"
                :class="
                  product.category === 'men\'s clothing'
                    ? 'bg-primary-white border-primary-blue color-primary-blue'
                    : 'bg-primary-white border-primary-purple color-primary-purple'
                "
              >
                Next product
              </button>
            </div>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script>
export default {
  name: "ProductDisplay",
  data() {
    return {
      product: {},
      index: 1,
      isLoading: false,
      isProductAvailable: true,
    };
  },
  methods: {
    async getProduct() {
      this.isLoading = true;
      try {
        const response = await fetch(
          `https://fakestoreapi.com/products/${this.index}`
        );
        const data = await response.json();
        if (
          data.category === "men's clothing" ||
          data.category === "women's clothing"
        ) {
          this.product = data;
          this.isProductAvailable = true;
        } else {
          this.isProductAvailable = false;
        }
      } catch (error) {
        console.log(error);
      }
      this.isLoading = false;
    },
    getNextProduct() {
      if (this.index === 20) {
        this.index = 1;
      } else {
        this.index++;
      }
      this.getProduct();
    },
    getRatingClass(n) {
      const rating = this.isProductAvailable ? this.product.rating.rate : 0;
      if (n <= rating) {
        return "fa-solid fa-circle";
      } else {
        return "fa-regular fa-circle";
      }
    },
  },
  mounted() {
    this.getProduct();
  },
};
</script>

<style scoped>
@import "@/assets/style/main.css";
</style>
