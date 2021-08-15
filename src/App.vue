<template>
  <navbar
    :cart="cart"
    :cart-total="cartTotal"
    :cart-qty="cartQty"
    @delete-item="deleteItem"
  />
  <div class="container">
    <router-view
      :products="products"
      :cart="cart"
      @add-item="addItem"
      @delete-item="deleteItem"
      :cart-total="cartTotal"
    />
  </div>
</template>

<script>
import Navbar from "@/components/Navbar";

export default {
  data: function () {
    return {
      cart: [],
      products: []
    };
  },
  components: {
    Navbar
  },
  created() {
    fetch("https://hplussport.com/api/products/order/price")
      .then(response => response.json())
      .then(data => {
        this.products = data;
      });
  },
  methods: {
    addItem(product) {
      let whichProduct;
      const existing = this.cart.filter(function (item, index) {
        if (Number(item.product.id) === Number(product.id)) {
          whichProduct = index;
          return true;
        } else {
          return false;
        }
      });

      if (existing.length > 0) {
        this.cart[whichProduct].qty++;
      } else {
        this.cart.push({ product, qty: 1 });
      }
    },
    deleteItem(id) {
      if (this.cart[id].qty > 1) {
        this.cart[id].qty--;
      } else {
        this.cart.splice(id, 1);
      }
    }
  },
  computed: {
    cartTotal() {
      let sum = 0;

      for (const item of this.cart) {
        sum += item.product.price * item.qty;
      }

      return sum;
    },
    cartQty() {
      let qty = 0;

      for (const item of this.cart) {
        qty += item.qty;
      }

      return qty;
    }
  }
};
</script>

<style lang="scss">
$primary: #6f42c1;
@import "node_modules/bootstrap/scss/bootstrap";
</style>
