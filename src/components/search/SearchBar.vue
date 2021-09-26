<template>
  <div class="search-bar">
    <div class="input-group">
      <input type="text" class="form-control" id="input-search" v-model="search_keyword" v-on:keydown="getSuggestions">
      <div class="input-group-append">
        <button type="button" class="btn btn-primary input-group-text" v-on:click="search">{{ texts.en.search }}</button>
      </div>
    </div>
    <autosuggest ref="Autosuggest"></autosuggest>
    <div class="row search-results-row" v-if="results"></div>
  </div>
</template>

<script>
import Autosuggest from "./SearchAutosuggest.vue";

let searchbar_component = {
  texts: {
    en: {
      'search-placeholder': 'Search product name',
      'search': 'Search'
    }
  },

};

/*
let watchers = {
  search_keyword: function(new_kw, old_kw) {
    console.log(old_kw + ' => ' + new_kw);
  }
}
*/


export default {
  name: "SearchBar",
  components: {
    Autosuggest
  },
  data() {
    return {
      products: require('../../data/products.json'),
      texts: searchbar_component.texts,
      //results: {},
      results: [],
      search_keyword: '',
      autosuggest_keywords: [],
      sort: 1,
    };
  },
//  watch: watchers,
  methods: {
    search: function() {
      this.getSuggestions(this.search_keyword);
      this.results = this.getSearchResults();

      if(this.sort === 1) {
        this.results = this.sortByRelevance(this.results);
      }
    },

    getSearchResults: function() {
      let keyword = this.search_keyword;

      return Object.values(this.products).filter(function(product) {
        let name = product.name;
        if(name !== '-' && name !== '') {
          if(name.toUpperCase().includes(keyword.toUpperCase())) {
            return true;
          }
        }
        return false;
      });
    },

    sortByRelevance: function(results) {
      let keyword = this.search_keyword.toUpperCase();
      return results.sort(function(product_a, product_b) {
        let pos_a = product_a.name.toUpperCase().indexOf(keyword);
        let pos_b = product_b.name.toUpperCase().indexOf(keyword);
        return pos_a - pos_b;
      });
    },
    getSuggestions() {
      return this.$refs.Autosuggest.getSuggestions(this.search_keyword);
    }
  }
};

</script>

<style>
  .input-group {
    position: relative;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -ms-flex-wrap: wrap;
    flex-wrap: wrap;
    -webkit-box-align: stretch;
    -ms-flex-align: stretch;
    align-items: stretch;
    width: 100%;
  }

  .input-group > .custom-file, .input-group>.custom-select, .input-group>.form-control {
    position: relative;
    -webkit-box-flex: 1;
    -ms-flex: 1 1 auto;
    flex: 1 1 auto;
    width: 1%;
    margin-bottom: 0;
  }
</style>
