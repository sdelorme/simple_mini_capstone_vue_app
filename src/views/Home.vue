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
      <p>{{ product.name }}</p>
      <p><button v-on:click="showProduct(product)">Show More Info</button></p>
      <dialog id="product-details">
        <form method="dialog">
          <h2>Product Information</h2>
          <p>
            Name:
            <input v-model="currentProduct.name" />
          </p>
          <p>
            Description:
            <input v-model="currentProduct.description" />
          </p>
          <p>
            Price: $
            <input v-model="currentProduct.price" />
          </p>
          <p>
            Image_url:
            <input v-model="currentProduct.image_url" />
          </p>
          <button v-on:click="updateProduct(currentProduct)">Update</button>
          <button v-on:click="destroyProduct(currentProduct)">Delete</button>
          <button>Close</button>
        </form>
      </dialog>
      <hr />
      <img v-bind:src="product.image_url" />
    </div>
  </div>
</template>
<style>
img {
  width: 200px;
}
</style>
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
      currentProduct: {},
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
    showProduct: function (theProduct) {
      console.log("Yay showing the thing");
      this.currentProduct = theProduct;
      document.querySelector("#product-details").showModal();
    },
    updateProduct: function (theProduct) {
      console.log("about to UPDATE this product though...");
      var params = {
        name: theProduct.name,
        description: theProduct.description,
        price: theProduct.price,
        image_url: theProduct.image_url,
      };
      axios.patch("http://localhost:3000/api/products/" + theProduct.id, params).then((response) => {
        console.log(response.data);
      });
    },
    destroyProduct: function (theProduct) {
      console.log(theProduct);
      axios.delete("http://localhost:3000/api/products/" + theProduct.id).then((response) => {
        console.log(response.data);
        var index = this.products.indexOf(theProduct);
        this.products.splice(index, 1);
      });
    },
  },
};
</script>
