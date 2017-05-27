<template>
  <div id="app">
    <paint> </paint>
  </div>
</template>

<script>
import paint from './paint.vue';

export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },

  components:{
              paint
  },
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Poppins');

#app {
  font-family: 'poppins', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;

}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
