<template>
  <div class="container mx-auto px-4">
    <div v-if="error" class="text-red-500">
      Sorry, an error occurred while fetching the products.
    </div>
    <div class="grid grid-cols-4 gap-4 row">
      <div v-for="product in products" :key="product.id" class="col-span-1 grid-container">
        <div class="card item1">
          <img :src="product.image" alt="product.title" class="card-img-top product-img">
          <div class="card-body">
            <h5 class="card-title">{{ product.title }}</h5>
            <p class="card-text">Category: {{ product.category }}</p>
            <p class="card-text">Price: {{ product.price }}</p>
            <button @click="addToCart(productId)" class="btn btn-primary">Add to Cart</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

    
import axios from 'axios';

const MARKUP_PERCENTAGE = 2.2;

export default {
  name: 'App',
  data() {
    return {
      products: [],
      cart: [],
      // error: false,
    };
  },
  methods: {
    async fetchProducts() {
      try {
        const response = await axios.get('https://fakestoreapi.com/products');
        this.products = response.data;

        // Add markup to each product
        this.products.forEach((product) => {
          product.price = product.price + product.price * MARKUP_PERCENTAGE / 100;
        });
      } catch (error) {
        this.error = true;
      }
    },
    addToCart(productId) {
      if (this.cart.includes(productId)) {
        alert("Product already exists inside the cart")
      } else {
        this.cart.push(productId);
        alert("Product added to cart")
      }
    },
  },
  created() {
    this.fetchProducts();
  },
};
</script> 



<style src="./tailwind.css">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container {
 
  margin-top: 20px;
  padding: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.card{
  width: 50%;
  margin: auto;
  height: auto;
  display: block;
  justify-content: center;
  align-items: center;
}
.card .product-img {
  width: 100px;
  padding-top: 40px;
  
}



.row {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-template-rows: 600px;
  grid-gap: 5px;
  background-color: #2196F3;
  padding: 10px;
}

.row > div {
  background-color: rgba(255, 255, 255, 0.8);
  text-align: center;
  padding: 20px 0;
  font-size: 20px;
}
button {
  display:block;
  padding:0.5em 1em 0.5em 1em;
  border-radius:100px;
  border:none;
  font-size:1em;
  position:relative;
  background:#0652DD;
  cursor:pointer;
  height:2em;
  width:8em;
  overflow:hidden;
  transition:transform 0.1s;
  z-index:1;
}
</style>
