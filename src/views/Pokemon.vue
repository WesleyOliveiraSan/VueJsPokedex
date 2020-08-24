<template>
  <div class="pokedex">
    <div class="header">
      <div id="mainCircle">
        <div></div>
      </div>
      <div class="circle" style="background-color: var(--red)"></div>
      <div class="circle" style="background-color: var(--yellow)"></div>
      <div class="circle" style="background-color: green"></div>
    </div>
    <div class="pokemon">
      <div class="image">
        <p>Hello {{poke.name | capitalize}} !</p>
        <img
          :src="frontImage ? poke.sprites.front_default : poke.sprites.back_default"
          @click="frontImage = !frontImage"
        />
        <span id="prev" @click="prevPokemon"></span>
        <span id="next" @click="nextPokemon"></span>
      </div>
      <div v-if="poke.stats.length > 0" class="description">
        <h2>{{poke.name | capitalize}}</h2>
        <span v-for="({type}) in poke.types" :key="type.name">{{type.name}}</span>
        <div class="hp">
          <span>HP</span>
          <div>{{poke.stats[0].base_stat}}</div>
        </div>
        <div class="defense">
          <span>DEF</span>
          <div>{{poke.stats[2].base_stat}}</div>
        </div>
        <div class="stats">
          <span>ATK</span>
          {{poke.stats[1].base_stat}}
        </div>
        <div class="stats">
          <span>SPC ATK</span>
          {{poke.stats[3].base_stat}}
        </div>
        <div class="stats">
          <span>SPC DEF</span>
          {{poke.stats[4].base_stat}}
        </div>
        <div class="stats">
          <span>SPD</span>
          {{poke.stats[5].base_stat}}
        </div>
      </div>
    </div>
    <div class="info">
      <div class="info-screen">
        <p>{{ability}}</p>
      </div>
      <div class="buttons">
        <button
          v-for="({ability}) in poke.abilities"
          :key="ability.name"
          @click="getAbility(ability.url)"
        >{{ability.name | capitalize}}</button>
      </div>
    </div>
  </div>
</template>

<script>
// Dependencies
import axios from 'axios';

export default {
  name: 'Pokemon',
  data() {
    return {
      frontImage: true,
      ability: 'Select an ability!',
      poke: {
        id: 0,
        name: '',
        abilities: [],
        sprites: {},
        stats: [],
        types: [],
      },
    };
  },
  created() {
    axios
      .get(`https://pokeapi.co/api/v2/pokemon/${this.$route.params.name}`)
      .then((res) => {
        this.poke.id = res.data.id;
        this.poke.name = res.data.name;
        this.poke.abilities = res.data.abilities;
        this.poke.sprites = res.data.sprites;
        this.poke.stats = res.data.stats;
        this.poke.types = res.data.types;
      });
  },
  methods: {
    getAbility(url) {
      axios.get(url).then((res) => {
        res.data.effect_entries.forEach(({ effect, language }) => {
          if (language.name === 'en') {
            this.ability = effect;
          }
        });
      });
    },
    prevPokemon() {
      axios
        .get(`https://pokeapi.co/api/v2/pokemon/${this.poke.id - 1}`)
        .then((res) => {
          this.poke = res.data;
        });
    },
    nextPokemon() {
      axios
        .get(`https://pokeapi.co/api/v2/pokemon/${this.poke.id + 1}`)
        .then((res) => {
          this.poke = res.data;
        });
    },
  },
  filters: {
    capitalize(value) {
      return value.charAt(0).toUpperCase() + value.slice(1);
    },
  },
};
</script>

<style lang="scss" scoped>
.pokedex {
  display: flex;
  flex-direction: column;
  max-width: 768px;
  margin: 0 auto;
  border: 12px solid var(--red);
  background-color: var(--red);
  border-radius: 16px;
  box-shadow: 2px 8px 16px #232323aa;

  .header {
    display: flex;
    align-items: flex-start;
    margin-bottom: 16px;
  }

  .pokemon {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    background-color: var(--dark-red);
    padding: 8px;
    border-radius: 6px;
    .image {
      position: relative;
      border: 6px solid #dedede;
      background-color: var(--white);
      border-radius: 16px 16px 16px 32px;
      display: flex;
      justify-content: center;
      align-items: center;
      width: 40%;
      img {
        width: 50%;
        transition: 0.3s;
        &:hover {
          transform: scale(1.1);
          cursor: pointer;
        }
      }
      p {
        position: absolute;
        top: 0;
        left: 0;
        color: var(--yellow);
        padding: 12px;
      }

      #prev,
      #next {
        transition: 0.3s;
        &:before,
        &:after {
          content: "";
          position: absolute;
          width: 0.9em;
          height: 3px;
          border-radius: 3px;
          background-color: #ccc;
        }
        &:hover {
          transform: scale(1.5);
          cursor: pointer;
        }
      }
      #prev {
        position: absolute;
        left: 10px;
        &:before {
          transform: rotateZ(45deg) translateX(5px);
        }
        &:after {
          transform: rotateZ(135deg) translateX(-5px);
        }
      }
      #next {
        position: absolute;
        right: 20px;
        &:before {
          transform: rotateZ(135deg) translateX(5px);
        }
        &:after {
          transform: rotateZ(45deg) translateX(-5px);
        }
      }
    }
    .description {
      width: 60%;
      padding: 8px;
      h2 {
        font-size: 2em;
        color: var(--white);
        margin-bottom: 6px;
      }
      > span {
        border-radius: 10px;
        font-size: 12px;
        padding: 2px 6px;
        text-align: center;
        letter-spacing: 1px;
        color: var(--white);
        background-color: var(--dark-yellow);
        margin-right: 4px;
        @media only screen and (min-width: 2560px) {
          font-size: 24px;
        }
      }

      .hp {
        margin: 8px 0px 4px 0px;
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: var(--dark);
        border-radius: 16px;
        > span {
          width: 7%;
          color: var(--dark-yellow);
          margin-left: 2px;
        }
        > div {
          width: 93%;
          background-color: #ff000088;
          border: 1px solid var(--white);
          border-radius: 16px;
          text-align: center;
        }
      }
      .defense {
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: var(--dark);
        border-radius: 16px;
        > span {
          width: 7%;
          color: var(--dark-yellow);
          margin-left: 2px;
        }
        > div {
          width: 93%;
          background-color: #dddddd88;
          border: 1px solid var(--white);
          border-radius: 16px;
          text-align: center;
        }
      }
      .stats {
        color: var(--white);
        > span {
          color: var(--dark-yellow);
          margin-left: 2px;
        }
      }
    }

    @media only screen and (max-width: 425px) {
      flex-direction: column;
      .image {
        width: 100%;
        img {
          width: 50%;
          margin: 10px 0px;
        }
      }
      .description {
        width: 100%;
        .hp {
          > span {
            width: 12%;
          }
          > div {
            width: 88%;
          }
        }
        .defense {
          > span {
            width: 12%;
          }
          > div {
            width: 88%;
          }
        }
      }
    }
  }

  .info {
    width: 90%;
    margin: 12px auto;
    .info-screen {
      display: flex;
      justify-content: center;
      align-items: center;
      background-color: var(--dark);
      color: green;
      border: 2px solid #111;
      border-radius: 16px;
      padding: 20px 10px;
      font-family: "Press Start 2P", cursive !important;
    }

    .buttons {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      margin: 12px 0px;
      grid-gap: 12px;
      > button {
        border: 1px solid white;
        border-radius: 12px;
        background-color: darken(#6bd0fa, 30%);
        color: white;
        padding: 14px 12px;
        font-size: 18px;
      }
      @media only screen and (max-width: 425px) {
        grid-template-columns: repeat(2, 1fr);
      }
    }
  }
  @media only screen and (min-width: 2560px) {
    max-width: 1440px;
    .info,
    .image > p,
    .description{
      font-size: 24px;
    }
  }
}
#mainCircle {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 60px;
  height: 60px;
  background-color: var(--white);
  border-radius: 50%;
  margin-right: 12px;
  > div {
    position: relative;
    width: 50px;
    height: 50px;
    background-color: var(--blue);
    border: 2px solid var(--dark);
    box-shadow: 1px 1px 10px #222;
    border-radius: 50%;
    &:before {
      content: "";
      width: 50%;
      height: 60%;
      border-radius: 50%;
      background-color: rgba(255, 255, 255, 0.7);
      border-radius: 50%;
      position: absolute;
      left: 5px;
      top: 5px;
    }
  }
}
.circle {
  position: relative;
  width: 15px;
  height: 15px;
  background-color: var(--red);
  border: 1px solid var(--dark);
  border-radius: 50%;
  margin-left: 4px;
  box-shadow: 2px 2px 6px #222;
  &:before {
    content: "";
    width: 30%;
    height: 40%;
    border-radius: 50%;
    background-color: rgba(255, 255, 255, 0.7);
    border-radius: 50%;
    position: absolute;
    left: 1.5px;
    top: 1.5px;
  }
}
</style>
