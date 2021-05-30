<template>
  <div class="container">
    <div v-if="pokemon">
      <p>Total{{ pokemon.count }}</p>
      <div v-for="result in pokemon.results" :key="result.id">
        <p>{{ result.name }}</p>
        <img
          :src="'https://lorempokemon.fakerapi.it/pokemon/200/' + result.name"
          alt=""
        />
      </div>

      <a :href="pokemon.next" target="_blank">next</a>
    </div>
  </div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      error: null,
      searchString: null,
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
  },
}
</script>

<style lang="scss" scoped>
.container {
  max-width: 1080px;
  width: 95%;
  // background-color: #edecf0;
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
</style>
