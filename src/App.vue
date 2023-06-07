<script>
  // donne à chaque todo un id unique
  let id = 0;

  import ChildComp from './components/ChildComposant.vue';
  import ChildCompProp from './components/ChildCompProp.vue';
  import ChildCompEmit from './components/ChildCompEmit.vue';
  import ChildCompSlot from './components/ChildCompSlot.vue'

  export default {
    // options du composant
    // déclarez du state réactif ici.
    data() {
      return {
        message: 'Vue.js 3!',
        className: 'red',
        titleClass: 'title',
        listClass: 'listTitle',
        ContainerClass: 'container',
        countClass: 'count',
        count: 0,
        text: '',
        awesome: true,
        newTodo: '',
        hideCompleted: false,
        todos: [
        { id: id++, text: 'Apprendre le HTML', done: true },
        { id: id++, text: 'Apprendre le JavaScript', done: true },
        { id: id++, text: 'Apprendre Vue', done: false }
        ],
        todoId: 1,
        todoData: null,
        propsChild:"prop passée !",
        childMsg: "Pas encore de message de l'enfant",
        msg: 'depuis le parent'
      }
    },
    components: {
      ChildComp,
      ChildCompProp,
      ChildCompEmit,
      ChildCompSlot
    },
    computed: {
      filteredTodos() {
        // retourne les todos filtrés en fonction de `this.hideCompleted`.
        return this.hideCompleted
          ? this.todos.filter((t) => !t.done)
          : this.todos
      }
    },
    mounted() {
      this.$refs.p.textContent = 'Le composant est maintenant monté!';
      this.fetchData();
    },
    watch: {
      todoId() {
        this.fetchData()
      }
    },
    methods: {
      sayHello() {
        alert('Bonjour, Vue.js 3!')
      },
      increment() {
        // met à jour le state du component
        this.count++
      },
      onInput(e) {
      // une directive v-on recoit un évènement natif du DOM
      // comme argument.
      this.text = e.target.value
      },
      toggle() {
        this.awesome = !this.awesome
      },
      addTodo() {
      this.todos.push({ id: id++, text: this.newTodo })
      this.newTodo = ''
      },
      removeTodo(todo) {
        this.todos = this.todos.filter((t) => t !== todo)
      },
      async fetchData() {
        this.todoData = null
        const res = await fetch(
          `https://jsonplaceholder.typicode.com/todos/${this.todoId}`
        )
        this.todoData = await res.json()
      },
    }
  }
</script>

<template>
  <div id="app">
    <img src="./assets/logo.png">
    <!-- <HelloWorld msg="Hello World!"/> -->
    <h1>Hello World!</h1>
    <h1>{{ message }} ({{ message.split('').reverse().join('') }})</h1>
    <!-- <p v-bind:class="className">Ceci est un paragraphe.</p> -->
    <!-- Vue.js permet de gérer facilement les événements sur les éléments
       HTML à l'aide de la directive "v-on".-->
       <!-- Une directive est un attribut spécial qui commence par le préfixe v-.
        Celles-ci font partie de la syntaxe du template de Vue. Similaire à l'interpolation du texte,
         les valeurs des directives sont des expressions en JavaScript qui ont accès
         au state du composant. -->
    <button v-on:click="sayHello" :class="titleClass">Cliquez ici</button>
    <!-- <h1 :class="titleClass">Passez moi en rouge</h1> -->
    <!-- De part son usage fréquent, v-on a une syntaxe abrégée : -->
    <button @click="increment" :class="countClass">compteur : {{ count }}</button>
    <!-- v-model synchronise automatiquement la valeur de l'<input> avec le state lié,
       donc nous n'avons plus besoin d'utiliser un gestionnaire d'événements pour cela. -->
    <!-- <p><input :value="text" @input="onInput" placeholder="Tapez ici" /></p> -->
    <p><input v-model="text" placeholder="Tapez ici"></p>
    {{console.log(text) }}
    <button @click="toggle">basculer</button>
    <h1 v-if="awesome">Vue est génial!</h1>
    <h1 v-else>Oh non 😢</h1>

    <div :class="ContainerClass">

      <!-- v-for pour render une liste d'éléments basée sur un tableau source -->
      <h1 :class="listClass">Rendu de liste</h1>

      <form @submit.prevent="addTodo">
        <input v-model="newTodo">
        <button>Ajouter une tâche</button>
      </form>
      <ul>
        <!-- Pour aiguiller Vue afin qu'il puisse tracer l'identité de chaque nœud,
         et ainsi réutiliser et réarranger l'ordre des éléments existants, 
         vous devez fournir un attribut key unique pour chaque todo -->
        <li v-for="todo in todos" :key="todo.id">
          <p>{{ todo.text }}
            <button @click="removeTodo(todo)">X</button>
          </p>
        </li>
      </ul>

      <h1 :class="listClass">Propriétés calculées</h1>
      <form @submit.prevent="addTodo">
        <input v-model="newTodo">
        <button>Ajouter une tâche</button>
      </form>
      <ul>
        <li v-for="todo in filteredTodos" :key="todo.id">
          <input type="checkbox" v-model="todo.done">
          <span :class="{ done: todo.done }">{{ todo.text }}</span>
          <button @click="removeTodo(todo)">X</button>
        </li>
      </ul>
      <button @click="hideCompleted = !hideCompleted">
        {{ hideCompleted ? 'Afficher toutes' : 'Cacher complétées' }}
      </button>

      <h1 :class="listClass">Cycle de vie et refs de template</h1>
      <p ref="p">A monter</p>

      <h1 :class="listClass">Observateurs</h1>
      <p>Id de la liste de tâches: {{ todoId }}</p>
      <button @click="todoId++">Récupérer la prochaine liste de tâches</button>
      <p v-if="!todoData">Chargement...</p>
      <pre v-else>{{ todoData }}</pre>

      <h1 :class="listClass">Composants</h1>
      <ChildComp />

      <h1 :class="listClass">Props</h1>
      <ChildCompProp />
      <ChildCompProp :msg="propsChild" />

      <h1 :class="listClass">Emits</h1>
      <!-- <ChildCompEmit /> -->
      <ChildCompEmit @response="(msg) => childMsg = msg"/>
      <p>{{ childMsg }}</p>

      <h1 :class="listClass">Slot</h1>
      <ChildCompSlot>
        Message: {{ msg }}
      </ChildCompSlot>
      

    </div>
    
  </div>
</template>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.title {
  color: red;
}
.count{
  color:green;
}
.container {
  text-align: left;

}
.listTitle{
  color:blueviolet; 
}
.done {
  text-decoration: line-through;
}
</style>
