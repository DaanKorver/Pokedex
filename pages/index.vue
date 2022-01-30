<template>
  <main>
    <ul>
      <PokedexEntry v-for="(mon, index) in this.pokemon" :key="index" :ref="mon" :id="mon.id" :name="mon.name" :types="mon.types" />
      <li class="fake" v-for="(mon, index) in this.pokemon" :key="index + 'f'"></li>
    </ul>
    <infinite-loading spinner="spiral" @infinite="this.infiteScroll">
      <div slot="spinner"><img class="spinner" src="~/static/spinner.svg" alt="pokeball"></div>
    </infinite-loading>
  </main>
</template>

<script>
import gsap from "gsap"
export default {
  data: ()=>{
    return {
      pokemon: [],
      url: 'https://pokeapi.co/api/v2/pokemon',
      next: 'https://pokeapi.co/api/v2/pokemon',
      detailPagePokemon: ''
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
      return pokemon
    },
    async fetchSinglePokemon(pokemonName) {
      const request = await this.$axios.get(`${this.url}/${pokemonName}`)
      const pokemon = request.data
      return pokemon
    }
  },
  transition: {
    self: this,
    css: false,
    mode: "out-in",
    leave(el, done) {
      document.body.style.overflow = "hidden"
      const pokemonEntry = document.querySelector(`.${this.$route.params.pokemon}`)
      const otherEntries = document.querySelectorAll(`.pokemon:not(.${this.$route.params.pokemon})`)
      const headerHeight = document.querySelector("header").clientHeight
      const clientRect = pokemonEntry.getClientRects()[0]
      const currentTop = getComputedStyle(pokemonEntry).getPropertyValue('top').slice(0, -2)
      const newTop = (currentTop - clientRect.top) + headerHeight
      
      gsap.to(pokemonEntry, {
        width: '100%',
        borderRadius: 0,
        top: newTop,
        height: '25vh',
        onComplete: function() {
          done()
          document.body.style.overflow = "auto"
        }
      })
      gsap.fromTo(`.${this.$route.params.pokemon} img`, {
        width: '6em',
        height: '6em',
      },
      {
        left: '50%',
        x: '-50%',
        bottom: 0,
        width: '10em',
        height: '10em'
      })
      gsap.to(otherEntries, {
        opacity: 0
      })
      gsap.to(`.${this.$route.params.pokemon} div`, {
        opacity: 0
      })
      // bottom: 246
      // height: 160
      // left: 127
      // right: 287
      // top: 86
      // width: 160
      // x: 127
      // y: 86
    }
  }
}
</script>

<style scoped>
.infinite-loading-container {
  text-align: left !important;
}

.spinner {
  position: relative;
  left: calc(50% - 2em);
  width: 4em;
  height: 4em;
  animation: spin 1s ease-in-out infinite;
}

ul {
  padding: 0 1em;
  position: relative;
}

li.fake {
  position: relative;
  height: 115px;
  margin: 1em 0;
  opacity: 0;
  pointer-events: none;
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
