<template>
  <!-- @ = on (event) -->
  <navbar
    :cart="cart"
    :cart-total="cartTotal"
    :cart-qty="cartQty"
    @delete-item="deleteItem"
  />
  <div class="container">
    <!-- Render the view applicable to the current path -->
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
import Navbar from "@/components/Navbar"; // @ = src

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
    // Executed when the view is created
    fetch("https://hplussport.com/api/products/order/price")
      .then(response => response.json())
      .then(data => {
        /* data: [{
         *   id: string (number),
         *   name: string,
         *   description: string,
         *   price: string (number),
         *   image_title: string (alt),
         *   image: string (src)
         * }]
         */
        this.products = data;
      });
  },
  methods: {
    addItem(product) {
      // Check if the product is already in the cart
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
        // If the item already exists in the cart, increment the quantity
        this.cart[whichProduct].qty++;
      } else {
        // If the item isn't in the cart, add the new item
        this.cart.push({ product, qty: 1 });
      }
    },
    deleteItem(id) {
      if (this.cart[id].qty > 1) {
        // Decrement the quantity of the item
        this.cart[id].qty--;
      } else {
        // Remove the item from the shopping cart
        this.cart.splice(id, 1);
      }
    }
  },
  computed: {
    // Properties derived from existing variables
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
