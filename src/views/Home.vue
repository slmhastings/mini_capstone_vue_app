<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div>
    <p>Name: <input type="text" v-model="newProductName"></p>
    <p>Description: <input type="text" v-model="newProductDescription"></p>
    <p>Price: <input type="text" v-model="newProductPrice"></p>
    <button v-on:click="createProducts">new product</button>
    <hr>
    </div>
    <!-- <button v-on:click="indexProducts">Products</button> -->

    <div v-for="product in products">
      <p>{{ product.name }}</p>
      <p>{{ product.description }}</p>
      <p>{{ product.price }}</p>
      <p>{{ product.image }}</p>
      <button v-on:click="showInfo(product)">More Info</button> 
      <hr />
    </div>

    <dialog id="product-details">
      <form method="dialog">
        <h1>Product Info</h1>
        <p>Name: {{ currentProduct.name }}</p>
        <p>Description: {{ currentProduct.description }}</p>
        <p>Price: {{ currentProduct.price }}</p>
        <button>Close</button>
      </form>
    </dialog>
    <!-- <h1>{{ products }}</h1> -->
  </div>
</template>
<style></style>
<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      products: [],
      currentProduct: {},
      newProductName: "",
      newProductDescription: "",
      newProductPrice: "",
    };
  },
  created: function () {
    console.log("I run");
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("/api/products").then((response) => {
        console.log(response);
        this.products = response.data;
      });
    },
    createProducts: function () {
      console.log("creating product...");
      var params = {
        name: this.newProductName,
        description: this.newProductDescription,
        price: this.newProductPrice,
      };
      axios.post("/api/products", params).then((response) => {
        console.log(response.data);
        this.products.push(response.data);
      });
    },
    showInfo: function (product) {
      console.log("Show Action");
      this.currentProduct = product;
      document.querySelector("#product-details").showModal();
    },
  },
};
</script>
