<template>
  <div>
    <h1>Teste de Aplicação Vue JS</h1>

    <input type="radio" value="True">
    <label>True</label>
    <br>

    <input type="radio" value="False">
    <label>False</label>
    <br>

    <button class="send" type="button">Send </button>
  </div>
</template>

<script>

export default {
  name: 'App',

  created(){
//     Vue.axios.get(api).then((response) => {
//   console.log(response.data)
// })

this.axios
.get('https://opentdb.com/api.php?amount=2&category=18&difficulty=medium&type=boolean')
.then((response) => {
  console.log(response.data.results[0])
})

// this.$http.get(api).then((response) => {
//   console.log(response.data)
// })
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;

  input[type=radio] {
    margin: 12px 4px;
  }

  button.send {
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 1rem;
    color: #fff;
    background-color: #1867c0;
    border: #1867c0;
    cursor: pointer;
  }
}
</style>
