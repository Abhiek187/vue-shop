<template>
  <nav class="navbar navbar-light sticky-top mr-3">
    <!-- Don't show the navbar if the cart is empty -->
    <div
      v-if="cart.length > 0"
      class="
        w-100
        navbar-text
        ms-auto
        d-flex
        justify-content-end
        position-relative
      "
    >
      <div
        class="
          me-3
          d-flex
          align-items-end
          flex-column
          bd-highlight
          mb-3
          position-absolute
        "
      >
        <div class="mb-2">
          <span class="fw-bold bg-white">
            <curr :amt="cartTotal" />
          </span>
          <button
            @click="toggleCartMenu"
            class="btn btn-sm btn-success ms-3"
            id="cartDropdown"
            aria-haspopup="true"
            aria-expanded="false"
          >
            <fa icon="shopping-cart" />
            {{ cartQty }}
          </button>
        </div>
        <cart-dropdown :cart="cart" :displayCart="displayCart" />
      </div>
    </div>
  </nav>
</template>

<script>
import Curr from "@/components/Curr";
import CartDropdown from "@/components/CartDropdown";

export default {
  data: function () {
    return {
      displayCart: false
    };
  },
  props: ["cart", "cartTotal", "cartQty"],
  components: {
    Curr,
    CartDropdown
  },
  methods: {
    toggleCartMenu() {
      this.displayCart = !this.displayCart;
    }
  }
};
</script>
