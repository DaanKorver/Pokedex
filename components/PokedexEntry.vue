<template>
  <li :class="[this.name, 'pokemon']" ref="entry" v-bind:style="[{backgroundColor : backgroundColor}, {top: top}]" @click="$router.push(`/pokemon/${name}`)">
    <div ref="info">
      <h2>{{ capitalizedName }}</h2>
      <p>#{{ id.toString().padStart(3, '0') }}</p>
      <ul class="types">
        <li v-for="(type, index) in types" :key="index">{{type.type.name}}</li>
      </ul>
    </div>
    <img :ref="this.name" :src="this.sprite" :alt="this.name">
  </li>
</template>

<script>
export default {
  props: ["id", "name", "types", "callback"],
  data: ()=>{
    return {
      isMounted: false,
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
    capitalizedName: function() {
      return this.name.charAt(0).toUpperCase() + this.name.slice(1)
    },
    backgroundColor: function() {
      return this.colors[this.types[0].type.name]
    },
    top: function() {
      if(!this.isMounted) return
      const height = this.$refs.entry.getClientRects()[0].height
      const top = ((this.id - 1) * 16) + (height * (this.id - 1))
      return `${top}px`
    }
  },
  mounted: function() {
    this.isMounted = true
  }
};
</script>

<style scoped>
  li.pokemon {
    color: white;
    display: flex;
    border-radius: .5em;
    padding: .5em;
    align-items: center;
    justify-content: space-between;
    position: absolute;
    width: calc(100% - 2em);
    left: 50%;
    transform: translateX(-50%);
    height: 115px;
  }

  div {
    display: flex;
    flex-direction: column;
  }

  img {
    width: 6em;
    height: 6em;
    object-fit: cover;
    position: absolute;
    right: .5em;
  }

  ul {
    display: flex;
    list-style-type: none;
    margin-top: 1em;
  }

  .types > li {
    background-color: rgba(0,0,0,.2);
    padding: .3em 1.3em;
    border-radius: 1em;
  }

  .types > li:first-child {
    margin-right: .5em;
  }

</style>