<template>
  <div class="dropdown-clip" v-if="cart.length > 0">
    <transition name="dropdown">
      <div
        v-if="displayCart"
        class="list-group bg-light border border-dark"
        aria-labelledby="cartDropdown"
      >
        <div v-for="(item, index) in cart" :key="index">
          <div class="dropdown-item-text text-nowrap text-end align-middle">
            <span class="badge bg-success align-text-top me-1">{{
              item.qty
            }}</span>
            {{ item.product.name }}
            <b>
              <curr :amt="item.qty * Number(item.product.price)" />
            </b>
            <!-- Tell the parent (Navbar) to trigger the deleteItem event with the current index -->
            <button
              @click.stop="this.$parent.$emit('deleteItem', index)"
              class="btn btn-sm btn-danger ms-2"
            >
              -
            </button>
          </div>
          <hr class="m-0" />
        </div>
        <!-- Link to navigate to checkout -->
        <router-link
          to="/checkout"
          class="btn btn-sm btn-success text-white float-end mt-2"
          >Checkout</router-link
        >
      </div>
    </transition>
  </div>
</template>

<script>
import Curr from "@/components/Curr";

export default {
  props: ["cart", "displayCart"],
  components: {
    Curr
  },
  emits: ["deleteItem"]
};
</script>

<style>
/* Animation to slide the dropdown down and up */
.dropdown-clip {
  overflow: hidden;
}

/* Rules while the animation is playing */
.dropdown-enter-active,
.dropdown-leave-active {
  transition: all 0.5s ease-in-out;
  transform: auto;
}

/* Rules when the animation begins and ends */
.dropdown-enter-from,
.dropdown-leave-to {
  opacity: 0;
  transform: translateY(-300px);
}
</style>
