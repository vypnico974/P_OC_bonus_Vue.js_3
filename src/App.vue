<script>
export default {
  // options du composant
  // déclarez du state réactif ici.
  data() {
    return {
      message: 'Vue.js 3!',
      className: 'red'
    }
  },
  methods: {
    sayHello() {
      alert('Bonjour, Vue.js 3!')
    }
  }
}
</script>

<template>
  <div id="app">
    <img src="./assets/logo.png">
    <!-- <HelloWorld msg="Hello World!"/> -->
    <h1>Hello World!</h1>
    <h1>{{ message }}</h1>
    <p v-bind:class="className">Ceci est un paragraphe.</p>
    <button v-on:click="sayHello">Cliquez ici</button>
  </div>
</template>


<!-- <script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
  
}
</script> -->

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
/* .id {
  color : red;
} */
</style>
