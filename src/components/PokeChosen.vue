<template>
  <div v-if="chosenPoke" class="chosen-box">
    <div class="chosen-title">
      <h1>{{ upperCase(chosenPoke.name) }}</h1>
      <img
        :src="pokeSprite(this.randomPokemon, 'other', 'official-artwork')"
        alt="Secret Pokemon"
      />
    </div>
    <div class="chosen-stats">
      <div class="chosen-stats-field chosen-type">
        <div class="chosen-type-1" v-if="chosenPoke.pokemon_v2_pokemontypes">
          <img
            :src="
              typeSprite(
                chosenPoke.pokemon_v2_pokemontypes[0].pokemon_v2_type.name
              )
            "
            alt="Secret Type"
          />
        </div>
        <div
          class="chosen-type-2"
          v-if="
            chosenPoke.pokemon_v2_pokemontypes &&
            chosenPoke.pokemon_v2_pokemontypes.length == 2
          "
        >
          <img
            :src="
              typeSprite(
                chosenPoke.pokemon_v2_pokemontypes[1].pokemon_v2_type.name
              )
            "
            alt="Secret Type"
          />
        </div>
      </div>
      <div class="chosen-stats-field chosen-gen">
        <p>GEN {{ chosenPoke.pokemon_v2_pokemonspecy.generation_id }}</p>
      </div>
      <div class="chosen-stats-field chosen-weight">
        <p>{{ chosenPoke.weight / 10 }}kg</p>
      </div>
      <div class="chosen-stats-field chosen-height">
        <p>{{ chosenPoke.height / 10 }}m</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PokeChosen',
  props: [
    'pokeList',
    'clickedPokemon',
    'randomPokemon',
    'pokeSprite',
    'upperCase',
    'typeSprite',
    'chosenPoke',
  ],
  data() {
    return {
      refresh: 0,
    };
  },
  computed: {
    testando() {
      return require('../assets/typeSprite/' + 'Bug' + '.svg');
    },
  },
};
</script>

<style lang="sass">
@import '../variables.sass'

.chosen-box
  margin: 20px
  width: 400px
  border-radius: 25px
  background: $chosenBackground
  display: flex
  flex-direction: column
  justify-content: center
  align-items: center
  box-shadow: 2px 2px 2px $shadow


.chosen-title
  display: flex
  flex-direction: column
  align-items: center
  h1
    margin: 5px
    font-family: $pixel
    font-weight: normal
  img
    width: 100px
    height: auto

.chosen-stats
  display: flex

.chosen-type
  display: flex
  margin: 10px
  align-items: center
  justify-content: center
  width: 120px
  .chosen-type-1
    height: 30px
    width: 30px
    margin: 0 10px
    img
      height: 30px
      width: 30px
  .chosen-type-2
    height: 30px
    width: 30px
    margin: 0 10px
    img
      height: 30px
      width: 30px

.chosen-stats-field
  background: $chosenBackgroundFields
  display: flex
  justify-content: center
  align-items: center
  height: 50px
  box-shadow: 2px 2px 2px $shadow
  margin: 10px 5px
  padding: 0 10px
  white-space: nowrap
  border-radius: 10px
  font-family: $pixel
  font-weight: normal
  text-shadow: 2px 2px 2px $shadow

.chosen-gen
  width: 75px

.chosen-weight
  width: 75px

.chosen-height
  width: 60px
</style>
