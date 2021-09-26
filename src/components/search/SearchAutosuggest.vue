<template>
  <div class="autosuggest" v-if="suggestions">
    <span v-for="(index, word) in suggestions" :key="word" class="autosuggest-word">
      {{ word }}
    </span>
  </div>
</template>

<script>
export default {
  name: 'SearchAutosuggest',
  data() {
    return {
      suggestions: []
    };
  },
  methods: {
    getSuggestions(keyword) {
      if(keyword.length > 1) {
        this.suggestions = [keyword + 'asd'];
      } else {
        this.suggestions = [];
      }
    }
  }
}


</script>

<style>
div.autosuggest {
  margin-top: 8px;
}

span.autosuggest-word {
    padding: 4px;
    background: rgb(0 123 255 / 45%);
    color: white;
    border-radius: 6px;
    margin-right: 6px;
}
</style>
