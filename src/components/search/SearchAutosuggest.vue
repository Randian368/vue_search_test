<template>
  <div class="autosuggest" v-if="suggestions.length" :style="dynamicStyle">
    <span class="autosuggest-prompt" :class="{ small: calculatedWidth < 300 }">{{ texts.prompt }}</span>
    <span v-for="word in suggestions" :key="word" class="autosuggest-word" v-on:click="addToSearchBar" v-on:mouseenter="decreaseOpacity" v-on:mouseleave="resetOpacity">
      {{ word }}
    </span>
  </div>
</template>

<script>
let searchautosuggest_component = {
  texts: {
    en: {
      'prompt': 'Did you mean: '
    },
    hu: {
      'prompt': 'Erre gondoltál?'
    }
  }
};

export default {
  name: 'SearchAutosuggest',
  el: '.autosuggest',
  data() {
    return {
      dynamicStyle: '',
      texts: searchautosuggest_component.texts[this.language],
      keywords: [],
      suggestions: []
    };
  },
  props: [
    'language',
    'calculatedWidth'
  ],
  watch: {
    calculatedWidth() {
      this.dynamicStyle += 'width: ' + this.calculatedWidth + 'px;';
    }
  },
  mounted() {
    this.keywords = require('../../data/keywords_' + this.language + '.json');
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
    },

    addToSearchBar(event) {
      let word = event.target.textContent

      this.$parent.search_keyword = word;
      this.$parent.$refs.inputSearchBar.value = word;

      this.suggestions = [];
    },

    decreaseOpacity(event) {
      event.target.style.background = "#007bffcc";
    },
    resetOpacity(event) {
      event.target.style.background = "#007bff73";
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
    padding: 1px 4px;
    background: #007bff73;
    color: white;
    border-radius: 6px;
    margin-right: 6px;
    margin-bottom: 3px;
    display: inline-block;
    cursor: pointer;
}

span.autosuggest-prompt {
  font-size: 0.9em;
  margin-right: 12px;
  opacity: 0.72;
}

span.autosuggest-prompt.small {
  position: relative;
  display: block;
  top: -6px;
}
</style>
