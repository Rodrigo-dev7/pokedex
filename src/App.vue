 <template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter is-flex-direction-row">
      <img src="./assets/pokeapi.png" />
      <hr />
      <h4 class="is-size-4">Pokedex</h4>
      <input
        class="input is-rounded"
        type="text"
        placeholder="Buscar pokemon pelo nome"
        v-model="busca"
      />
      <button
        class="button is-medium is-fullwidth button is-info"
        id="buscarBtn"
        @click="buscar"
      >
        Buscar
      </button>

      <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="index + 1" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon";

export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: "",
    };
  },
  created: function () {
    axios
      .get(`https://pokeapi.co/api/v2/pokemon?limit=152&offset=0`)
      .then((res) => {
        console.log("Lista de Pokemons");
        this.pokemons = res.data.results;
        this.filteredPokemons = res.data.results;
        console.log(this.filteredPokemons);
      });
  },
  components: {
    Pokemon,
  },
  methods: {
    buscar: function () {
      this.filteredPokemons = this.pokemons;
      if (this.busca == "" || this.busca == " ") {
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca);
        
      }
    },
  },
  computed: {
    // resultadoBuscar: function () {
    //   if (this.buscar == "" || this.buscar == " ") {
    //     return this.pokemons;
    //   } else {
    //     return this.pokemons.filter((pokemon => pokemon.name == this.buscar))
    //   }
    // },
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

#buscarBtn {
  margin-top: 2%;
}
</style>
