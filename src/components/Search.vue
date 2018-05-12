<template>
  <div class="search">
    <h1>{{msg}}</h1>
    <form @submit.prevent="getResult(query)">
      <input type="text" v-model="query" placeholder="Enter your search...">
    </form>
    <div class="results" v-if="results">
      <div class="result" v-for="result in results" v-bind:key="result.links[0].title">
        <img :src="result.links[0].href" :alt="result.data[0].title">
        <p>{{result.data[0].title}}</p>
      </div>
    </div>
    
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'search',
  data () {
    return {
      msg: 'Search the stars with Vue.js and the Nasa API!',
      query: null,
      results: '',
    }
  },
  methods: {
    getResult(query) {
      axios.get('https://images-api.nasa.gov/search?q=' + query + '&media_type=image').then(response => {
        console.log(response.data.collection.items);
        this.results = response.data.collection.items;
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  input {
    width: 50%;
    height: 2rem;
    padding-left: 1rem;
  }
  .results {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    align-items: center;
    max-width: 1024px;
    margin: 2rem auto;
  }
  .result {
    flex-basis: 100%;
  }
  img {
    width: 100%;
  }
  @media screen and (min-width: 768px){
      .result {
        flex-basis: 32%;
      }
  }
  @media screen and (min-width: 1024px){
    .result {
      flex-basis: 23%;
    }
  }
</style>
