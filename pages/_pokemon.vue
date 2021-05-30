<template>
  <div v-if="pokemon" class="container">
    <h1>{{ pokemon.name }}</h1>
    <div class="img-container">
      <img
        :src="
          'https://lorempokemon.fakerapi.it/pokemon/200/' + pokemon.sprites.id
        "
        alt=""
      />
    </div>

    <div class="information">
      <p>Experice:{{ pokemon.base_experience }}</p>
      <p>Height:{{ pokemon.height }}</p>
      <p>Weight:{{ pokemon.weight }}</p>
    </div>
    <div v-for="(item, id) in pokemon.moves" :key="id" class="name">
      <p>{{ item.move.name }}</p>
    </div>
    <div v-for="(item, id) in pokemon.stats" :key="id">
      <p style="color: blue">Hello{{ item.stats }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      error: null,
      searchString: null,
      pokemon: null,
    }
  },
  created() {
    this.fetchPokemons()
  },
  methods: {
    fetchPokemons() {
      const config = {
        method: 'get',
        url: `https://pokeapi.co/api/v2/pokemon/${this.$route.params.pokemon}`,
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
  background-color: #edecf0;
  width: 95%;
  margin: 0 auto;
  h1 {
    text-align: center;
  }
  .img-container {
    width: 20rem;
    margin: 0 auto;

    img {
      width: 100%;
      margin: 0 auto;
    }
  }
}
</style>
