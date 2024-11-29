<template>

  <div class="container mt-4 rounded-5 custom-bg">
    <br>
    <h1 class="text-center mb-4" style="color:white">Shopping Cart</h1>
    <div class="row g-3">
      <form @submit.prevent="addProduct" class="row mb-3">
        <input v-model="newProduct.name" placeholder="Product" class="form-control col" required/>
        <input v-model.number="newProduct.quantity" type="number" placeholder="Qty" class="form-control col" required/>
        <input v-model.number="newProduct.price" type="number" step="0.01" placeholder="Price" class="form-control col"
               required/>
        <button class="btn btn-primary col btn-lg" style="background-color: darkslategrey; border-color: white;">Add</button>
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
          <button @click="removeProduct(item.name)" class="btn btn-danger" style="background-color:yellowgreen; border-color: antiquewhite;">Remove</button>
        </td>
      </tr>
      </tbody>
    </table>
    <p style="font-size: 20px"> Tax: 22% </p>
    <h3 class="mt-4">Total: ${{ cartTotal.toFixed(2) }}</h3>
    <br>
    <br>


    <form>
      <div class="row content">
        <div class="col-md">
          <input type="text" id="fname" name="fname" placeholder="Enter discount code"><br>
        </div>
        <div class="col-sm-10">
          <input type="submit" id="discountButton" class="btn btn-warning" value="Apply discount!">
        </div>
      </div>


    </form>
    <br>
    <br>
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
        axios.get(${this.api}/cart-items).then(res => (this.cartItems = res.data)),
        axios.get(${this.api}/total).then(res => (this.cartTotal = res.data)),
      ]);
    },
    addProduct() {
      axios.post(${this.api}/add-product, this.newProduct).then(this.fetchCart);
      this.newProduct = {name: "", price: 0, quantity: 1};
    },
    removeProduct(name) {
      axios.delete(${this.api}/delete-item/${name}).then(this.fetchCart);
    },
  },
  mounted() {
    this.fetchCart();
  },
};
</script>

<style>
.custom-bg {
  background-color: rebeccapurple; opacity: 0.5;
}
</style>