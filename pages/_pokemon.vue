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
      <p v-for="(item, id) in pokemon.moves" :key="id" class="name">
        {{ item.move.name }}
      </p>
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
  background-color: #edecf0;
  margin: 0 auto;
  height: 100vw;
  h1 {
    text-align: center;
    text-transform: capitalize;
  }
  .img-container {
    max-width: 18rem;
    margin: 0 auto;
    background-color: #fff;
    border-radius: 5px;

    img {
      display: flex;
      flex-direction: row;
      justify-content: center;
      align-content: center;
      width: 95%;
      padding: 5% 0;
      margin: 1rem auto;
    }
  }
  .information {
    text-transform: capitalize;
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-content: center;
    p {
      margin-left: 0.5rem;
      background-color: #fff;
      padding: 8px 16px;
      border-radius: 5px;
    }
  }
}
</style>
