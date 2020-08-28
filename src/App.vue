<template>
  <div id="app">
    <label for="input-area">
      <input
        type="text"
        id="input-area"
        v-model="query"
        @keyup.enter="getQuery"
      >
    </label>
    <div class="queryFromSpace" v-if="parsedData[0]">
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
      query: '',
      parsedData: {},
    };
  },
  methods: {
    getQuery() {
      if (this.query) {
        console.log(this.query);
        fetch(`${this.api_url}/${this.query}`)
          .then((res) => {
            if (!res.ok) {
              return `${this.query} — is a wrong query! No Data found!`;
            }
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.query = '';
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
