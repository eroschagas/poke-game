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
      return 2;
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

.console
  position: fixed
  cursor: pointer

.pokelist
  height: 100vh
  width: 55%
  margin: 0 10px
.section-1
  // display: flex
  // flex-direction: column
  width: 45%
</style>
