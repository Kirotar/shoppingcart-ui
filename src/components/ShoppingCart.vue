<template>
  <div class="container mt-4 bg-primary-subtle rounded-5">
    <br>
    <h1 class="text-center mb-4">Shopping Cart</h1>
    <div class="text-center p-3 mb-2 bg-secondary-subtle text-secondary-emphasis">Hello, John!</div>
    <div class="row g-3">
      <form @submit.prevent="addProduct" class="row mb-3">
        <input v-model="newProduct.name" placeholder="Product" class="form-control col" required/>
        <input v-model.number="newProduct.quantity" type="number" placeholder="Qty" class="form-control col" required/>
        <input v-model.number="newProduct.price" type="number" step="0.01" placeholder="Price" class="form-control col"
               required/>
        <button class="btn btn-primary col btn-lg">Add</button>
      </form>
    </div>
    </div>
  <div class="container mt-4">

    <table class="table">
      <thead>
      <tr>
        <th>Product</th>
        <th>Quantity</th>
        <th>Price</th>
        <th>Action</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="item in cartItems" :key="item">
        <td>{{ item.name }}</td>
        <td>{{ item.quantity }}</td>
        <td>${{ item.price }}</td>

        <td>
          <button @click="removeProduct(item.name)" class="btn btn-success">Remove</button>

        </td>

      </tr>
      </tbody>
    </table>

    <h3 class="mt-4">Total: ${{ cartTotal.toFixed(2) }}</h3>

    <div class="dropdown">
      <a class="btn btn-danger dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
        Apply discount </a>

      <ul class="dropdown-menu">
        <li><a class="dropdown-item" href="#">SUMMR</a></li>
        <li><a class="dropdown-item" href="#">Christmas 2024</a></li>
        <li><a class="dropdown-item" href="#">New Client</a></li>
      </ul>
    </div>
    <br>
    <br>
  </div>

  <div class="container">
    <div class="row content">
      <div class="col-sm-4 bg-info">col-sm-4</div>
      <div class="col-sm-4 bg-warning">col-sm-4</div>
      <div class="col-sm-4 bg-danger">col-sm-4</div>
    </div>
    <div class="row content">
      <div class="col-sm-2 bg-info">col-sm-2</div>
      <div class="col-sm-4 bg-warning">col-sm-4</div>
      <div class="col-sm-6 bg-danger">col-sm-6</div>
    </div>
    <div class="row content">
      <div class="col-sm-2 bg-info">col-sm-2</div>
      <div class="col-sm-5 bg-warning">col-sm-5</div>
      <div class="col-sm-5 bg-danger">col-sm-5</div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: () => ({
    api: "http://localhost:8089/api/cart",
    newProduct: {name: "", price: 0, quantity: 1},
    cartItems: [],
    cartTotal: 0,
  }),
  methods: {
    fetchCart() {
      axios.all([
        axios.get(`${this.api}/cart-items`).then(res => (this.cartItems = res.data)),
        axios.get(`${this.api}/total`).then(res => (this.cartTotal = res.data)),
      ]);
    },
    addProduct() {
      axios.post(`${this.api}/add-product`, this.newProduct).then(this.fetchCart);
      this.newProduct = {name: "", price: 0, quantity: 1};
    },
    removeProduct(name) {
      axios.delete(`${this.api}/delete-item/${name}`).then(this.fetchCart);
    },
  },
  mounted() {
    this.fetchCart();
  },
};
</script>