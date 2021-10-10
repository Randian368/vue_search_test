<template>
  <div class="search-bar">
    <div class="input-group">
      <input type="text" ref="inputSearchBar" class="form-control input-search" v-model="search_keyword" v-on:keyup="getSuggestions">
      <div class="input-group-append">
        <button type="button" class="btn btn-primary input-group-text" v-on:click="search">{{ texts.search }}</button>
      </div>
    </div>
    <autosuggest ref="Autosuggest" v-bind:language="language" v-bind:calculated-width="inputSearchWidth" v-on:autosuggest_accepted="search"></autosuggest>
    <div class="row search-results-row" :class="{ small: inputSearchWidth < 300 }" v-if="results.length">
      <product-snippet v-for="product in results" :key="product.product_id" v-bind:product="product"></product-snippet>
    </div>
  </div>
</template>

<script>
import Autosuggest from "./SearchAutosuggest.vue";
import ProductSnippet from "../product/ProductSnippet.vue";

let searchbar_component = {
  texts: {
    en: {
      'search-placeholder': 'Search product name',
      'search': 'Search'
    },
    hu:  {
      'search-placeholder': 'Terméknév vagy tulajdonság',
      'search': 'Keresés'
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
    Autosuggest,
    ProductSnippet
  },
  data() {
    return {
      products: require('../../data/products_' + this.language + '.json'),
      texts: searchbar_component.texts[this.language],
      //results: {},
      results: [],
      search_keyword: '',
      autosuggest_keywords: [],
      sort: 1,
      inputSearchWidth: 0,
    };
  },
  mounted() {
    this.inputSearchWidth = this.$refs.inputSearchBar.offsetWidth;
  },
  props: [
    'language'
  ],
//  watch: watchers,
  methods: {
    search() {
      this.results = this.getSearchResults();

      if(this.sort === 1) {
        this.results = this.sortByRelevance(this.results);
      }
    },

    getSearchResults() {
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

    sortByRelevance(results) {
      let keyword = this.search_keyword.toUpperCase();
      return results.sort(function(product_a, product_b) {
        let pos_a = product_a.name.toUpperCase().indexOf(keyword);
        let pos_b = product_b.name.toUpperCase().indexOf(keyword);
        return pos_a - pos_b;
      });
    },

    getSuggestions() {
      return this.$refs.Autosuggest.getSuggestions(this.search_keyword, this.language);
    }
  }
};

</script>

<style>
  .search-bar {
    z-index: 10;
  }
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

  .search-results-row {
    background-color: #f4f4f4f4;
    border: 1px solid #f4f4f4f4;
    margin: 0px;
    padding-top: 16px;
    max-height: 70vh;
    overflow-y:auto;
  }
</style>
