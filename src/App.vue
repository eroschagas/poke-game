<template>
  <div class="console" @click="teste">console.log</div>

  <PokeChosen
    :pokeList="pokeList"
    :randomPokemon="randomPokemon"
    :clickedPokemon="clickedPokemon"
    :pokeSprite="pokeSprite"
    :upperCase="upperCase"
    :typeSprite="typeSprite"
    :chosenPoke="chosenPoke"
    :pokeFound="pokeFound"
  />
  <PokeList
    :pokeList="pokeList"
    :pickPokemon="pickPokemon"
    :pokeSprite="pokeSprite"
    :chosenPoke="chosenPoke"
    :pokeFind="pokeFind"
    :pokeMissed="pokeMissed"
  />
</template>

<script>
import PokeList from './components/PokeList.vue';
import PokeChosen from './components/PokeChosen.vue';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    PokeList,
    PokeChosen,
  },
  data() {
    return {
      pokeList: [],
      clickedPokemon: [],
      pokeFound: false,
    };
  },
  async created() {
    await this.getPokemon();
  },
  computed: {
    maxPokemon() {
      return this.pokeList.filter(
        (p) =>
          p.is_default &&
          JSON.parse(p.pokemon_v2_pokemonsprites[0].sprites).front_default
      ).length;
    },
    randomPokemon() {
      return Math.floor(Math.random() * this.maxPokemon);
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
        JSON.parse(this.pokeList[0].pokemon_v2_pokemonsprites[0].sprites)
      );
    },

    async getPokemon() {
      try {
        const result = await axios({
          method: 'POST',
          url: 'https://beta.pokeapi.co/graphql/v1beta',
          data: {
            query: `
              {
                pokemon_v2_pokemon {
                  id
                  name
                  height
                  weight
                  is_default
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
              }
            `,
          },
        });
        this.pokeList = result.data.data.pokemon_v2_pokemon;
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
    pickPokemon(index) {
      this.clickedPokemon.push(index);
    },
    pokeFind(pick) {
      if (this.chosenPoke == pick && this.clickedPokemon.includes(pick)) {
        this.pokeFound = true;
        return true;
      }
      this.pokeFound = false;
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
.console
  position: fixed
  cursor: pointer
</style>
