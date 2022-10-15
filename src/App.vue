<template>
  <PokeList :pokeList="pokeList" />
</template>

<script>
import PokeList from './components/PokeList.vue';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    PokeList,
  },
  data() {
    return {
      pokeList: [],
    };
  },
  async created() {
    await this.getPokemon();
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
  },
};
</script>

<style lang="sass"></style>
