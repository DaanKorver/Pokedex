<template>
  <section ref="entry">
    <div>
      <p>#{{ id.toString().padStart(3, '0') }}</p>
      <h2>{{ capitalizedName }}</h2>
      <ul>
        <li v-for="(type, index) in types" :key="index">{{type.type.name}}</li>
      </ul>
    </div>
    <img :src="this.sprite" :alt="this.name">
  </section>
</template>

<script>
export default {
  props: ["id", "name", "types"],
  data: ()=>{
    return {
      spriteUrl: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/"
    }
  },
  computed: {
    sprite: function() {
      return `${this.spriteUrl}${this.id}.png`
    },
    capitalizedName: function() {
      return this.name.charAt(0).toUpperCase() + this.name.slice(1)
    }
  }
};
</script>

<style scoped>
  section {
    color: white;
    display: flex;
    border-radius: .5em;
    padding: .5em;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1em;
    background: lightgray;
  }

  div {
    display: flex;
    flex-direction: column;
  }

  img {
    width: 40%;
  }

  ul {
    display: flex;
    list-style-type: none;
    margin-top: 1em;
  }

  ul > li {
    background-color: rgba(0,0,0,.2);
    padding: .3em 1.3em;
    border-radius: 1em;
  }

  ul > li:first-child {
    margin-right: .5em;
  }

</style>