<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>Product: <input type="text" v-model="newProductParams.name"> </p>
    <p>Price: <input type="text" v-model="newProductParams.price"> </p>
    <p>image_url: <input type="text" v-model="newProductParams.image_url"> </p>
    <button v-on:click="indexProducts()">Create Product</button>
    <div v-for="product in products" v-bind:key="product.id">
      <p>id: {{ product.id }}</p>
      <p>name: {{ product.name }}</p>
      <p>Price: {{ product.price }}</p>
      <p>Image Url: {{ product.image_url }}</p>
      <img v-bind:src="product.image_url">
    </div>
  </div>
</template>
<style>
img {
  width: 250px;
  height: 250px;
}
</style>
<style></style>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      message2: "Welcome again",
      products: [],
      newProductParams: {},
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:3000").then((response) => {
        console.log(response.data);
        this.products = response.data;
      });
    },
    createProduct: function () {
      var productParams = {
        price: this.newProductParams.price,
        image_url: this.newProductParams.image_url,
        name: this.newProductParams.name,
      };
      axios.post("http://localhost3000/products", productParams).then((response) => {
        console.log(response.data);
        this.products.push(response.data);
        this.newProductParams = {};
      });
    },
  },
};
</script>