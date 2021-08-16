<template>
  {{ dollar(amt) }}
</template>

<script>
export default {
  props: ["amt"],
  methods: {
    dollar(value) {
      // Convert a string to a dollar value
      return "$" + Number.parseFloat(value).toFixed(2);
    }
  }
};
</script>
