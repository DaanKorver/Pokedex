<template>
  <main>
    <PokedexEntry v-for="(mon, index) in this.pokemon" :key="index" :id="mon.id" :name="mon.name" />
    <infinite-loading :spinner="'spiral'" @infinite="this.infiteScroll">
      <div slot="no-more">No more Pokémon</div>
      <div slot="no-results">No more Pokémon</div>
      <div slot="spinner"><img class="spinner" src="~/static/spinner.svg" alt="pokeball"></div>
    </infinite-loading>
  </main>
</template>

<script>
export default {
  data: ()=>{
    return {
      pokemon: [],
      url: 'https://pokeapi.co/api/v2/pokemon',
      next: 'https://pokeapi.co/api/v2/pokemon',
    }
  },
  methods: {
    async infiteScroll($state) {
      const pokemon = await this.fetchPokemonData($state)
      for (let i = 0; i < pokemon.length; i++) {
        const mon = pokemon[i]
        const monData = await this.fetchSinglePokemon(mon.name)
        const {id, name, types} = monData
        if(id > 1000) {
          $state.complete()
          return
        }
        this.pokemon.push({id, name, types})
      }
      $state.loaded()
    },
    async fetchPokemonData($state) {
      if(!this.next) {
        $state.complete()
        return
      }
      const request = await this.$axios.get(this.next)
      this.next = request.data.next ? request.data.next : false
      const pokemon = request.data.results
      console.log(pokemon);
      return pokemon
    },
    async fetchSinglePokemon(pokemonName) {
      const request = await this.$axios.get(`${this.url}/${pokemonName}`)
      const pokemon = request.data
      return pokemon
    }
  }
}
</script>

<style>
  .spinner {
    width: 4em;
    height: 4em;
    animation: spin 1s ease-in-out infinite;
  }

  @keyframes spin {
    0% {
      transform: rotate(0deg);
    }

    100% {
      transform: rotate(359deg);
    }
  }
</style>
