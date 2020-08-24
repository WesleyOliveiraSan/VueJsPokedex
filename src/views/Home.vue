<template>
  <div id="home">
    <form @submit="searchPokemon($event)">
      <input type="text" v-model="search" placeholder="Search..." />
      <button>Catch' Em!</Button>
    </form>
    <div id="pokemons">
      <Pokemon v-for="pokemon in filtered" :key="pokemon.name" :pokemon="pokemon" />
    </div>
    <div id="loadMore">
      <button @click="loadMore">Load More</button>
    </div>
  </div>
</template>

<script>
// Dependencies
import axios from 'axios';

// Components
import Pokemon from '@/components/Pokemon.vue';

export default {
  name: 'Home',
  components: {
    Pokemon,
  },
  data() {
    return {
      offset: 0,
      search: '',
      pokemons: [],
      filtered: [],
    };
  },
  created() {
    axios
      .get('https://pokeapi.co/api/v2/pokemon?limit=100&offset=0')
      .then((res) => {
        this.pokemons = res.data.results;
        this.filtered = this.pokemons;
      });
  },
  computed: {
    filteredPokemons() {
      if (this.search.trim() === '') {
        return this.pokemons;
      }
      return this.pokemons.filter((poke) => poke.name.search(this.search.toLowerCase()) >= 0);
    },
  },
  methods: {
    searchPokemon($event) {
      $event.preventDefault();
      this.filtered = this.filteredPokemons;
    },
    loadMore() {
      this.offset += 100;
      axios
        .get(`https://pokeapi.co/api/v2/pokemon?limit=100&offset=${this.offset}`)
        .then((res) => {
          this.pokemons = this.pokemons.concat(res.data.results);
          this.filtered = this.pokemons;
          console.log(this.pokemons);
        });
    },
  },
};
</script>

<style lang="scss" scoped>
#home {
  form {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    margin-bottom: 15px;
    input {
      border: 1px solid var(--blue);
      color: var(--blue);
      width: 25%;
      font-size: 18px;
      text-align: center;
      border-radius: 16px;
      &::placeholder{
        color: var(--blue);
      }
    }
    button{
      background-color: var(--blue);
      color: var(--white);
      margin-left: 8px;
      padding: 4px 8px;
      border: 0;
      border-radius: 16px;
    }
    @media only screen and (max-width: 425px) {
      grid-template-columns: repeat(2, 1fr);
      input {
        width: 75%;
      }
      button {
        width: 25%;
        padding: 4px 0px;
      }
    }
  }
  #pokemons {
    display: grid;
    grid-gap: 1em;
    @media only screen and (min-width: 1440px) {
      grid-template-columns: repeat(5, 1fr);
    }
    @media only screen and (max-width: 1440px) {
      grid-template-columns: repeat(4, 1fr);
    }
    @media only screen and (max-width: 768px) {
      grid-template-columns: repeat(3, 1fr);
    }
    @media only screen and (max-width: 425px) {
      grid-template-columns: repeat(2, 1fr);
    }
    @media only screen and (max-width: 320px) {
      grid-template-columns: repeat(1, 1fr);
    }
  }
  #loadMore{
    display: flex;
    justify-content: center;
      margin-top:15px;
    button{
      background-color: var(--blue);
      font-size: 18px;
      color: var(--white);
      margin-left: 8px;
      padding: 4px 8px;
      border: 0;
      border-radius: 16px;
    }

  }
}
</style>
