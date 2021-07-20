<template>

  <div class="home">
    
    <header>Access-Control-Allow-Origin: *</header>
    <h1>{{ message }}</h1>
    <p>Product: <input type="text" v-model="newProductParams.name"> </p>
    <p>Description: <input type="text" v-model="newProductParams.description" ></p>
    <p>Price: <input type="text" v-model="newProductParams.price"> </p>
    <p>image_url: <input type="text" v-model="newProductParams.image_url"> </p>
    <button v-on:click="createProduct()">Create Product</button>
    <div v-for="product in products" v-bind:key="product.id">
      <p>Product: {{ product.id }}</p>
      <p>name: {{ product.name }}</p>
      <p>description: {{ product.description }}</p>
      <p>Price: {{ product.price }}</p>
      <p>Image Url: {{ product.image_url }}</p>
      <img v-bind:src="product.image_url">
      <p><button v-on:click="productsCreate()">Show more info</button></p>
    </div>
    <p><button v-on:click="showProduct(product)">Show more info</button></p>
    <dialog id="product-details">
      <form method="dialog">
        <p>Product: <input type="text" v-model="newProductParams.id"></p>
        <p>Price: <input type="text" v-model="newProductParams.price"></p>
        <p>Description: <input type="text" v-model="newProductParams.description"></p>
        <p>img_url: <input type="text" v-model="newProductParams.img_url"></p>
        <button>close</button>
        <button v-on:click="updateProduct(currentProduct)"> Update Product </button>
      </form>
    </dialog>
  </div>
</template>
<style>
img {
  width: 250px;
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
      currentProduct: {},
      theProduct: {},
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:3000/products").then((response) => {
        this.products = response.data;
      });
    },
    createProduct: function () {
      var productParams = {
        description: this.newProductParams.description,
        price: this.newProductParams.price,
        image_url: this.newProductParams.image_url,
        name: this.newProductParams.name,
      };
      axios.post("http://localhost3000/products", productParams).then((response) => {
        this.products.push(response.data);
        this.newProductParams = {};
      });
    },
    showProduct: function (theProduct) {
      this.currentProduct = theProduct;
      console.log("showing product");
      document.querySelector("#product-details").showModal();
    },
    updateProduct: function (theProduct) {
      var editParams = theProduct;
      axios.patch("http://localhost:3000/products/" + editParams.id, editParams).then((response) => {
        console.log(response.data);
      });
    },
    destroyProduct: function (theProduct) {
      axios.delete("http://localhost:3000/products/${theProduct.id}").then((response) => {
        var index = this.products.indexOf(theProduct);
        this.products.splice(index, 1);
      });
    },
  },
  computed: {
    sortedProducts: function () {
      var sortProducts = this.products;
      return sortProducts.sort((a, b) => a.id - b.id);
    },
  },
};
</script>