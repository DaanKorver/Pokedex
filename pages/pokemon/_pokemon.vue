<template>
  <section>
    <header v-bind:style="{backgroundColor : backgroundColor}">
      <img ref="pokemon" :src="this.sprite" alt="">
    </header>
  </section>
</template>

<script>
  export default {
    data: ()=>{
      return {
        spriteUrl: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/",
        colors: {
          grass: "#a9cf98",
          fire: "#f5a66b",
          water: "#a7c7e7",
          bug: "#a9cf98",
          normal: "#d2d3c6",
          poison: "#bfb2d7",
          electric: "#f2cb56",
          ground: "#ead0b7",
          fairy: "#fcd5d6",
          fighting: "#b47a4e",
          psychic: "#e7a8d8",
          rock: "#ead0b7",
          ghost: "#bfb2d7",
          dark: "#404040",
          ice: "#779ecb",
          dragon: "#9382e3",
          steel: "#b3b4c2",
          flying: "#a7c7e7"
        }
      }
    },
    computed: {
      sprite: function() {
        return `${this.spriteUrl}${this.id}.png`
      },
      backgroundColor: function() {
        return this.colors[this.types[0].type.name]
    }
    },
    async asyncData({ params, $axios }) {
      const pokemonName = params.pokemon 
      const request = await $axios.get(`https://pokeapi.co/api/v2/pokemon/${pokemonName}`)
      const pokemon = request.data
      const {id, name, types} = pokemon
      return { id,name,types }
    }
  }
</script>

<style scoped>
  header {
    height: 25vh;
    position: relative;
    background: transparent;
  }
  img {
    width: 10em;
    height: 10em;
    position: absolute;
    left: 50%;
    bottom: 0;
    transform: translateX(-50%);
  }
</style>