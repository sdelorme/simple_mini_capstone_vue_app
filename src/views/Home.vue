<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p><button v-on:click="createProduct">Create New Product</button></p>
    <p>
      Name
      <input v-model="newProductName" />
    </p>
    <p>
      Description
      <input v-model="newProductDescription" />
    </p>
    <p>
      Price
      <input v-model="newProductPrice" />
    </p>
    <p>
      Image Url
      <input v-model="newProductImageUrl" />
    </p>
    <div v-for="product in products" v-bind:key="product.id">
      <hr />
      {{ product.name }}
      <hr />
    </div>
  </div>
</template>
<style></style>
<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js! Where all your dreams come true....",
      products: [],
      newProductName: "",
      newProductDescription: "",
      newProductPrice: "",
      newProductImageUrl: "",
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:3000/api/products").then((response) => {
        console.log(response.data);
        this.products = response.data;
      });
    },
    createProduct: function () {
      console.log("about to create this product though...");
      var params = {
        name: this.newProductName,
        description: this.newProductDescription,
        price: this.newProductPrice,
        image_url: this.newProductImageUrl,
      };
      axios.post("http://localhost:3000/api/products", params).then((response) => {
      console.log(response.data);
      this.products.push(response.data);
      this.newProductName = "";
      this.newProductDescription = "";
      this.newProductPrice = "";
      this.newProductImageUrl = "";
      });
    },
  },
};
</script>
