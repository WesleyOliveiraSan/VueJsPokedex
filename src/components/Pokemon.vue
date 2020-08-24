<template>
  <router-link :to="`/pokemon/${pokemon.name}`">
    <div class="card">
      <div class="card-img">
        <div class="tags">
          <span v-for="({type}) in poke.types" :key="type.name">{{type.name}}</span>
        </div>
        <img :src="poke.img" />
      </div>
      <div class="card-content">
        <p>{{pokemon.name | capitalize}}</p>
      </div>
    </div>
  </router-link>
</template>

<script>
import axios from 'axios';

export default {
  props: {
    pokemon: Object,
  },
  data() {
    return {
      poke: {
        types: [],
        img: '',
      },
    };
  },
  created() {
    axios.get(this.pokemon.url).then((res) => {
      this.poke.types = res.data.types;
      this.poke.img = res.data.sprites.front_default;
    });
  },
  filters: {
    capitalize(value) {
      return value.charAt(0).toUpperCase() + value.slice(1);
    },
  },
};
</script>

<style lang="scss" scoped>
.card {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border: 1px solid #dedede;
  border-radius: 8px;
  background-color: var(--red);
  box-shadow: 2px 6px 6px #dedede;
  height: 15em;

  .card-img {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 60%;

    background-color: var(--white);
    border-radius: 6px 6px 0 0;

    .tags {
      position: absolute;
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      grid-gap: 6px;
      top: 0;
      left: 0;
      margin: 4px;

      span {
        border-radius: 10px;
        font-size: 12px;
        padding: 2px 6px;
        text-align: center;
        letter-spacing: 1px;
        color: var(--white);
        background-color: var(--dark-yellow);
      }
    }
  }
  .card-content {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 40%;
    background-color: var(--red);
    border-radius: 6px;
    p {
      font-size: 1.6em;
      color: var(--white);
    }
  }
}
</style>
