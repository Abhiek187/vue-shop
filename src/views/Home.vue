<template>
  <section class="container">
    <!-- Create a two-way binding between the max variable here and in RangeSelector -->
    <range-selector :products="filteredProducts" v-model="max" />
    <product-list :products="filteredProducts" />
  </section>
</template>

<script>
import ProductList from "@/components/ProductList";
import RangeSelector from "@/components/RangeSelector";

export default {
  name: "Home", // optional
  data: function () {
    return {
      max: 50, // starting slider value
      cart: []
    };
  },
  props: ["products"],
  components: {
    RangeSelector,
    ProductList
  },
  computed: {
    filteredProducts() {
      // Only show the products within a price range
      return this.products.filter(item => item.price < Number(this.max));
    }
  }
};
</script>
