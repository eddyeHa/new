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

  components: {
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

button:focus, input:focus {
  outline: none;
}

/* ------------------------------------- */
/* ------------ TRANSITIONS ------------ */
/* ------------------------------------- */

.enterUp-item {
  display: inline-block;
  margin-right: 10px;
}

.enterUp-enter-active /*, .list-enter-leave */ {
  transition: all 0.5s;
}

.enterUp-enter, .enterUp-leave-to {
  opacity: 0;
  transform: translateY(30px);
}

.fade-enter-active {
  transition: opacity 0.3s
}

/* Removed for visual reasons
.fade-leave-active {
  transition: opacity 0.3s
}
*/

.fade-enter, .fade-leave-active {
  opacity: 0
}


</style>
