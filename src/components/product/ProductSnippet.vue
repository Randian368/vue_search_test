<template>
  <div class="product-snippet">
    <div class="image">
      <img v-bind:src="'https://newoc3.drdkft.hu/image/' + product.image">
    </div>
    <div class="prod-attribute name">{{ product.name }}</div>
    <div v-if="product.quantity > 0" class="in-stock-badge"><span>{{ product.quantity }}</span> in stock!</div>
    <div class="prod-attribute data">
      <table>
        <tbody>

        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductSnippet",
  props: {
    product: Object
  }
};
</script>

<style scoped>
  div.product-snippet {
    position: relative;
    width: calc(100% - 8px);
    background-color: white;
    padding: 8px;
    margin-bottom: 24px;
    margin-left: 4px;
    box-shadow: 1px 1px 1px #e9ecef;
    border: 1px solid white;
    overflow-y:hidden;
  }

  div.product-snippet:hover {
    cursor:pointer;
    border:1px solid #caa67a;
  }

  div.image {
    width: fit-content;
    display: inline-block;
  }
  div.image > img {
    width: 160px;
    min-height: 80px;
    display:inline-block;
  }
  div.prod-attribute {
    width: calc(100% - 200px);
    display:inline-block;
    margin-left: 12px;
    position:relative;
  }
  div.name {
    position: absolute;
    top: 4px;
    font-size: 0.85em;
    font-weight: 500;
    color: hsl(210deg 8% 24%);
  }

  div.in-stock-badge {
    width: fit-content;
    border-radius: 6px;
    background: rgb(255 234 51);
    position: relative;
    padding: 2px 4px;
    top: 12px;
    font-size: 0.76em;
    font-weight: 700;
    text-transform: uppercase;
  }



</style>
