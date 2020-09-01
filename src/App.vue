<template>
  <div id="app">
    <label for="input-area">
      <input
        type="text"
        id="input-area"
        v-model="query"
        @keyup.enter="parseTheQuery"
      >
    </label>
    <div class="parsedQueryFromSpace"
         v-if=" parsedData "
    >
      {{ parsedData }}
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      api_url: 'https://api.spacexdata.com/v3',
      // query from input
      query: '',
      // object where will be data add
      parsedData: '',
      // parsed query at the good type
      parsedQuery: '',
      // dragonsInfo: 'Info about dragons: ',
      // maybe i should create JSON for queries for lightweight code
      dragonsArray: ['dragon', 'drakon', 'dragons', 'drag'],
    };
  },
  methods: {
    parseTheQuery() {
      for (let i = 0; i < this.dragonsArray.length; i += 1) {
        const elem = this.dragonsArray[i];
        if (this.query === elem) {
          this.parsedQuery = 'dragons';
          break;
        }
        // it needs to see other queries, not only dragons....
      }
      return this.getQuery();
    },
    getQuery() {
      console.log(this.parsedQuery);
      if (this.parsedQuery) {
        fetch(`${this.api_url}/${this.parsedQuery}`)
          .then((res) => {
            if (!res.ok) {
              console.log('ne ok');// how come it doesn't go to this place?
              return `${this.parsedQuery} — is a wrong query! No Data found!`;
            }
            return res.json();
          })
          .then(this.setResults);
        this.query = '';
      }
      console.warn('Out of the if state');
      this.parsedData = '';
      return this.parsedData;
    },
    setResults(results) {
      this.parsedQuery = '';
      this.parsedData = results;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
