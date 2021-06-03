<template>
  <div class="container">
    <h1>Pokemon</h1>
    <div class="search-bar">
      <input
        v-model="searchString"
        type="text"
        placeholder="search your pokemon"
      />
      <button @click="searchPokemon" class="btn">Submit</button>
    </div>
    <template v-if="searchPokemon">
      <div v-if="pokemon" class="pokemon">
        <div v-for="result in pokemon.results" :key="result.id" class="card">
          <a :href="'http://localhost:3000' + pokemon.id" target="_blank"
            ><img
              :src="
                'https://lorempokemon.fakerapi.it/pokemon/200/' + result.name
              "
              alt=""
            />
            <p>{{ result.name }}</p></a
          >
        </div>
        <p>Total{{ pokemon.count }}</p>
        <a :href="pokemon.next" target="_blank">next</a>
      </div>
    </template>
    <template v-if="!searchPokemon">
      <div v-if="pokemon" class="pokemon">
        <div v-for="result in pokemon.results" :key="result.id" class="card">
          <a :href="'http://localhost:3000' + pokemon.id" target="_blank"
            ><img
              :src="
                'https://lorempokemon.fakerapi.it/pokemon/200/' + result.name
              "
              alt=""
            />
            <p>{{ result.name }}</p></a
          >
        </div>
        <p>Total{{ pokemon.count }}</p>
        <a :href="pokemon.next" target="_blank">next</a>
      </div>
    </template>
  </div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      error: null,
      searchString: null,
      searchPokemon: null,
      pokemon: null,
    }
  },
  computed: {
    classObject() {
      return {
        active: this.isActive && !this.error,
        'text-danger': this.error && this.error.type === 'fatal',
      }
    },
  },
  mounted() {
    this.fetchPokemons()
    this.searchPokemons()
  },
  methods: {
    fetchPokemons() {
      const config = {
        method: 'get',
        url: `https://pokeapi.co/api/v2/pokemon`,
      }
      this.$axios(config)
        .then((response) => {
          console.log(response.data)
          this.pokemon = response.data
        })
        .catch(function (error) {
          console.log(error)
        })
    },
    searchPokemons() {
      const config = {
        method: 'get',
        url: `https://pokeapi.co/api/v2/pokemons=${this.searchString}`,
      }
      this.$axios(config)
        .then((response) => {
          this.searchPokemon = response.data
        })
        .catch(function (error) {
          console.log(error)
        })
    },
  },
}
</script>

<style lang="scss" scoped>
.container {
  background-color: #edecf0;
  width: 100vw;
  min-height: 100vh;
  h1 {
    text-align: center;
    padding-top: 1rem;
    margin-bottom: 1rem;
  }
  .search-bar {
    width: 30%;
    margin: 0 auto;

    input {
      padding: 0.5rem 3rem;
      border-top-left-radius: 5px;
      border-bottom-left-radius: 5px;
    }
    input:focus {
      outline: none;
    }
    .btn {
      padding: 0.5rem 2rem;
      transform: translateX(-5px);
      border-top-right-radius: 5px;
      border-bottom-right-radius: 5px;
    }
  }
  .pokemon {
    max-width: 1080px;
    width: 95%;
    margin: 2rem auto 0 auto;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
    grid-gap: 1rem;
    @media (max-width: 768px) {
      display: grid;
      grid-template-columns: 1fr;
      grid-gap: 1rem;
    }
    .card {
      width: 100%;
      margin: 0 auto;
      background-color: #ffff;
      border: 1px solid #d2d7ee;
      border-radius: 5px;
      @media (max-width: 768px) {
        width: 90%;
        margin: 0 auto;
      }
      a {
        text-align: center;
        text-decoration: none;
        text-transform: capitalize;
        color: #000;
        img {
          width: 90%;
          margin: 0.75rem auto 0 auto;
          display: flex;
          flex-direction: column;
          justify-content: center;
        }
        p {
          padding-bottom: 0.75rem;
          font-weight: 400;
          touch-action: pan-x;
        }
      }
    }
  }
}
</style>
