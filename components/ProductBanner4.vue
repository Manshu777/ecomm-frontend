<template>
    <div class="white-cards">
    
      <div class="white-card" v-for="(card, index) in cards" :key="index">
        <div class="category_titles">
             <h5>Men's Trendy Fashion | Up to 55% off</h5>
           </div>
  
        <div class="card-row">
          <div v-for="(product, productIndex) in card.products.slice(0, 2)" :key="productIndex" class="card-img">
            <nuxt-link :to="`${product.slug}/product/${product.id}`">
            <img :src="'http://localhost:8000/uploads/' + product.image" class="product_img" />
            </nuxt-link>
          </div>
        </div>
        <div class="card-row">
          <div v-for="(product, productIndex) in card.products.slice(2)" :key="productIndex" class="card-img">
            <nuxt-link :to="`${product.slug}/product/${product.id}`">
            <img :src="'http://localhost:8000/uploads/' + product.image" class="product_img" />
            </nuxt-link>
          </div>
        </div>
        <div class="link-card" >
         
         
           <nuxt-link to="/all/mens-wear">
           See all
            </nuxt-link>
  
        </div>
      </div>
  
    </div>
  </template>
  <script>
  export default {
    name: 'ProductsSlider',
    data() {
      return {
        productList: [],
        loading: true,
        cards: []
      }
    },
    async mounted() {
      try {
      const response = await this.$axios.$get('https://admin.swascabs.com/api/v1/all?category=mens-wear');
      const products = response.data.result.data;
      this.loading = false;
  
      // Group products into cards
      let cards = [];
      const limitedProducts = products.slice(0, 16);
      for (let i = 0; i < limitedProducts.length; i += 4) {
        cards.push({ products: products.slice(i, i + 4) });
      }
      this.cards = cards;
  
      console.log(this.cards);
   } catch (error) {
      console.error('Error fetching product data:', error);
   }
    
    }
  }
  </script>
  
  
  <style scoped>
  .white-cards {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    gap: 10px;
    margin-top: 10px;
    margin-left: 10px;
  }
  
  .white-card {
    background-color: #fff;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    margin-bottom: 20px;
    flex: 1;
  }
  
  .card-row {
    display: flex;
      margin-bottom: 10px;
      align-items: center;
      justify-content: space-around
  }
  
  /* .card-img {
    flex: 1;
    margin-right: 10px;
  }
  
  .card-img  {
    max-width: 100%;
    height: auto;
    display: block;
  } */
  
   .link-card{
    font-size: 1.2rem;
      font-weight: 500;
      color: #007185;
   }
  
  .card-img  img {
    flex: 1;
      width: 140px;
      height: 140px;
      margin-left: 4px;
      object-fit: scale-down;
      gap: 5px;
    }
    
    .card-img:hover {
     transform: scale(1.1);
     transition: transform 0.3s ease-in-out;
    }
    .category_titles{
      font-size: 1.2rem;
      line-height: 1.8rem;
      margin-bottom: 10px;
    }
  </style>
  