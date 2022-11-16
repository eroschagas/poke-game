<template>
  <div class="home">
    <div class="console" @click="teste">console.log</div>

    <div class="section-1">
      <PokeChosen
        :loadingList="loadingList"
        :pokeList="pokeList"
        :randomPokemon="randomPokemon"
        :clickedPokemon="clickedPokemon"
        :pokeSprite="pokeSprite"
        :upperCase="upperCase"
        :typeSprite="typeSprite"
        :chosenPoke="chosenPoke"
        :pokeFound="pokeFound"
      />
      <PokePick
        :typeSprite="typeSprite"
        :clickedPokemon="clickedPokemon"
        :pokeSprite="pokeSprite"
        :chosenPoke="chosenPoke"
      />
    </div>
    <PokeList
      class="pokelist"
      :loadingList="loadingList"
      :pokeList="pokeList"
      :pickPokemon="pickPokemon"
      :pokeSprite="pokeSprite"
      :chosenPoke="chosenPoke"
      :pokeFind="pokeFind"
      :pokeFound="pokeFound"
      :pokeMissed="pokeMissed"
    />
  </div>
</template>

<script>
import axios from 'axios';
import PokeList from './templates/PokeList.vue';
import PokeChosen from './templates/PokeChosen.vue';
import PokePick from './templates/PokePick.vue';

export default {
  name: 'App',
  components: {
    PokeList,
    PokeChosen,
    PokePick,
  },
  data() {
    return {
      pokeList: [],
      clickedPokemon: [],
      pokeFound: false,
      loadingList: true,
    };
  },
  async created() {
    await this.getPokemon();
  },
  watch: {
    clickedPokemon: {
      deep: true,
      handler() {
        if (this.clickedPokemon.includes(this.chosenPoke)) {
          this.pokeFound = true;
        } else {
          this.pokeFound = false;
        }
      },
    },
  },
  computed: {
    maxPokemon() {
      return this.pokeList.filter(
        (p) => JSON.parse(p.pokemon_v2_pokemonsprites[0].sprites).front_default
        // JSON.parse(p.pokemon_v2_pokemonsprites[0].sprites).other.dream_world
        //   .front_default
      );
    },
    randomPokemon() {
      // return Math.floor(Math.random() * this.maxPokemon);
      return 5;
    },
    chosenPoke() {
      if (this.pokeList) {
        return this.pokeList[this.randomPokemon];
      }
      return {};
    },
  },
  methods: {
    teste() {
      console.log(this.clickedPokemon);
      console.log(this.pokeList);
      console.log(
        JSON.parse(
          this.pokeList[this.randomPokemon].pokemon_v2_pokemonsprites[0].sprites
        )
      );
    },

    async getPokemon() {
      this.loadingList = true;
      try {
        const result = await axios({
          method: 'POST',
          url: 'https://beta.pokeapi.co/graphql/v1beta',
          data: {
            // eslint-disable-next-line
            query: `{ pokemon_v2_pokemon(where: {is_default: {_eq: true}, pokemon_v2_pokemonsprites: {sprites: {_lt: "{\\\"front_default\\\": null"}}}, order_by: {id: asc}) {
                  id
                  name
                  height
                  weight
                  pokemon_v2_pokemontypes {
                    slot
                    pokemon_v2_type {
                      name
                    }
                  }
                  pokemon_v2_pokemonspecy {
                    generation_id
                  }
                  pokemon_v2_pokemonsprites {
                    sprites
                  }
                }
              }`,
          },
        });
        this.pokeList = result.data.data.pokemon_v2_pokemon;
        setTimeout(() => {
          this.loadingList = false;
        }, 2000);
      } catch (error) {
        console.error(error);
      }
    },
    typeSprite(type) {
      return require('./assets/typeSprite/' + this.upperCase(type) + '.svg');
    },
    pokeSprite(index = 0, type = 'front_default', second_type, third_type) {
      if (this.pokeList.length) {
        switch (type) {
          case 'other':
            var jsonOther = JSON.parse(
              this.pokeList[index].pokemon_v2_pokemonsprites[0].sprites
            )[type][second_type].front_default;
            return jsonOther;

          case 'versions':
            var jsonVersions = JSON.parse(
              this.pokeList[index].pokemon_v2_pokemonsprites[0].sprites
            )[type][second_type][third_type].front_default;
            return jsonVersions;

          default:
            var jsonDefault = JSON.parse(
              this.pokeList[index].pokemon_v2_pokemonsprites[0].sprites
            )[type];
            return jsonDefault;
        }
      }
    },
    upperCase(string) {
      return string.charAt(0).toUpperCase() + string.slice(1);
    },
    pickPokemon(item) {
      if (!this.pokeMissed(item) && !this.pokeFound) {
        this.clickedPokemon.push(item);
      }
    },
    pokeFind(pick) {
      if (this.chosenPoke == pick && this.clickedPokemon.includes(pick)) {
        return true;
      }
      return false;
    },
    pokeMissed(pick) {
      if (this.chosenPoke != pick && this.clickedPokemon.includes(pick)) {
        return true;
      }
      return false;
    },
  },
};
</script>

<style lang="sass">
.home
  display: flex
  // background-color: #FFFFFF
  // background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='498' height='59.8' viewBox='0 0 1000 120'%3E%3Cg fill='none' stroke='%23FF0000' stroke-width='1.3' stroke-opacity='0.37'%3E%3Cpath d='M-500 75c0 0 125-30 250-30S0 75 0 75s125 30 250 30s250-30 250-30s125-30 250-30s250 30 250 30s125 30 250 30s250-30 250-30'/%3E%3Cpath d='M-500 45c0 0 125-30 250-30S0 45 0 45s125 30 250 30s250-30 250-30s125-30 250-30s250 30 250 30s125 30 250 30s250-30 250-30'/%3E%3Cpath d='M-500 105c0 0 125-30 250-30S0 105 0 105s125 30 250 30s250-30 250-30s125-30 250-30s250 30 250 30s125 30 250 30s250-30 250-30'/%3E%3Cpath d='M-500 15c0 0 125-30 250-30S0 15 0 15s125 30 250 30s250-30 250-30s125-30 250-30s250 30 250 30s125 30 250 30s250-30 250-30'/%3E%3Cpath d='M-500-15c0 0 125-30 250-30S0-15 0-15s125 30 250 30s250-30 250-30s125-30 250-30s250 30 250 30s125 30 250 30s250-30 250-30'/%3E%3Cpath d='M-500 135c0 0 125-30 250-30S0 135 0 135s125 30 250 30s250-30 250-30s125-30 250-30s250 30 250 30s125 30 250 30s250-30 250-30'/%3E%3C/g%3E%3C/svg%3E")

.console
  position: fixed
  cursor: pointer

// .pokelist
//   height: 100vh
//   width: 55%
//   margin: 0 10px
.section-1
  // display: flex
  // flex-direction: column
  width: 45%
</style>
