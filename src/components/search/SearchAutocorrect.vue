<template>
  <div class="autocorrect">
    <span v-for="(index, word) in results" :key="word" class="autocorrect-word">{{ word }}</span>
  </div>
</template>

<script>



</script>

<style>
div.autocorrect {
  margin-top: 8px;
}

span.autocorrect-word {
    padding: 4px;
    background: rgb(0 123 255 / 45%);
    color: white;
    border-radius: 6px;
    margin-right: 6px;
}
</style>
