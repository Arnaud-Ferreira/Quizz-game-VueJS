<template>

  <div>

   <h1>Title of the question</h1>
    <div class="inputs">
   <input type="radio" name="options" value="True">
   <label>True</label><br>

   <input type="radio" name="options" value="False">
   <label>False</label><br><button class="send" type="button">Send</button>

    </div>

  </div>
</template>

<script>

export default {
  name: 'App',
  
  created() {
    this.axios
    .get('https://opentdb.com/api.php?amount=10&category=18')
    .then((response) => {
      console.log(response.data.results[0])
    })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;
}

#app input[type=radio] {
  margin: 12px 4px;
}

button.send {
  margin-top: 15px;
  height: 40px;
  min-width: 120px;
  padding: 0 16px;
  color: #fff;
  background-color: #3e7ec7;
  border: 1px solid #3e7ec7;
  cursor: pointer;
  border-radius: 3px;
}
</style>
