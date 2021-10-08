<template>
  <div class="autosuggest" v-if="suggestions">
    <span v-for="word in suggestions" :key="word" class="autosuggest-word">
      {{ word }}
    </span>
  </div>
</template>

<script>
export default {
  name: 'SearchAutosuggest',
  data() {
    return {
      keywords: [],
      suggestions: []
    };
  },
  props: [
    'language'
  ],
  mounted() {
    this.keywords = require('../../data/keywords_' + this.language + '.json')
  },
  methods: {
    getSuggestions(search_phrase) {
      this.suggestions = [];
      let suggestions = [];
      if(search_phrase.length > 2) {
        for(let keyword of this.keywords) {
          if(keyword.toLowerCase().startsWith(search_phrase.toLowerCase())) {
            suggestions[suggestions.length] = {word: keyword, priority: 1};
          } else if(keyword.toLowerCase().includes(search_phrase.toLowerCase())) {
            suggestions[suggestions.length] = {word: keyword, priority: 2};
          }
        }

        if(suggestions.length > 0) {
          suggestions.sort((a, b) => {
            if(a.priority === b.priority) {
              return 0;
            } else {
              return (a.priority < b.priority) ? -1 : 1;
            }
          });
          this.suggestions = suggestions.map((x) => {
            return x.word;
          });
        }
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
