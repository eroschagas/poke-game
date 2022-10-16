<template>
  <PokeChosen
    :pokeList="pokeList"
    :randomPokemon="randomPokemon"
    :clickedPokemon="clickedPokemon"
    :pokeSprite="pokeSprite"
    :upperCase="upperCase"
    :typeSprite="typeSprite"
    :chosenPoke="chosenPoke"
  />
  <PokeList
    :pokeList="pokeList"
    :pickPokemon="pickPokemon"
    :pokeSprite="pokeSprite"
    :chosenPoke="chosenPoke"
    :pokeFound="pokeFound"
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
    pokeFound(pick) {
      if (
        this.chosenPoke.id == pick.id &&
        this.clickedPokemon.includes(pick.id)
      ) {
        return true;
      }
      return false;
    },
    pokeMissed(pick) {
      if (
        this.chosenPoke.id != pick.id &&
        this.clickedPokemon.includes(pick.id)
      ) {
        return true;
      }
      return false;
    },
  },
};
</script>

<style lang="sass"></style>
