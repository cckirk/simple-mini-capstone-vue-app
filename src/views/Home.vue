<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="(table, index) in tables" :key="table.id">
      <p>{{index}}. {{table.name}}</p>
      <p>{{index}}. {{table.price }}</p>
    </div>
  </div>
</template>
<style></style>
<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      message2: "Welcome again",
      tables: [
        {id: 1, name: "Product 1", price: 10},
        {id: 2, name: "Product 2", price: 230},
      ],
    };
  },
  created: function() {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:8080").then(response => { console.log(response.data);
        this.products = response.data;
      })
    }
  },
};
</script>
