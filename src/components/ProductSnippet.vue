<template>
  <div class="product-snippet">
    <div class="image">
      <img v-bind:src="'https://newoc3.drdkft.hu/image/' + product.image">
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductSnippet",
  props: {
    product: Object,
    title: String
  }
};
</script>

<style scoped>
  div.product-snippet {
    position: relative;
    width: calc(100% - 32px);
    background-color: #f4f4f4f4;
    padding: 8px;
    margin-left: 16px;
  }
  img {
    width: 160px;
    display:inline-block;
  }


</style>
