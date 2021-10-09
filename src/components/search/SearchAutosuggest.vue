<template>
  <div class="autosuggest" v-if="suggestions.length" :style="dynamicStyle">
    <span v-for="word in suggestions" :key="word" class="autosuggest-word">
      {{ word }}
    </span>
  </div>
</template>

<script>
export default {
  name: 'SearchAutosuggest',
  el: '.autosuggest',
  data() {
    return {
      dynamicStyle: '',
      keywords: [],
      suggestions: []
    };
  },
  props: [
    'language',
    'calculatedWidth'
  ],
  mounted() {
    this.keywords = require('../../data/keywords_' + this.language + '.json');
  },
  watch: {
    calculatedWidth() {
      this.dynamicStyle += 'width: ' + this.calculatedWidth + 'px;';
    }
  },
  methods: {
    getSuggestions(search_phrase) {
      this.suggestions = [];
      let suggestions = [];
      if(search_phrase.length > 1) {
        for(let keyword of this.keywords) {
          if(keyword.toLowerCase().startsWith(search_phrase.toLowerCase())) {
            suggestions[suggestions.length] = {word: keyword, priority: 1 + keyword.length};
          } else if(keyword.toLowerCase().includes(search_phrase.toLowerCase())) {
            suggestions[suggestions.length] = {word: keyword, priority: 200 + keyword.length};
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
  background: hsl(211deg 40% 50% / 6%);
  z-index: 100;
  padding: 6px;
  padding-top: 12px;
  border-radius: 3px;
  position: relative;
  top: -1px;
}

span.autosuggest-word {
    padding: 4px;
    background: rgb(0 123 255 / 45%);
    color: white;
    border-radius: 6px;
    margin-right: 6px;
    margin-bottom: 3px;
    display: inline-block;
}
</style>
