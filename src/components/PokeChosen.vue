<template>
  <div v-if="chosenPoke" class="chosen-box">
    <div v-if="pokeFound" class="chosen-title">
      <h1>{{ upperCase(chosenPoke.name) }}</h1>
      <img
        :src="pokeSprite(this.randomPokemon, 'other', 'official-artwork')"
        alt="Secret Pokemon"
      />
    </div>
    <div class="chosen-stats">
      <div class="chosen-stats-field chosen-type">
        <div>
          <div
            class="chosen-type-show fadeIn-swirl-grow"
            v-if="chosenPoke.pokemon_v2_pokemontypes && type1"
          >
            <img
              :src="
                typeSprite(
                  chosenPoke.pokemon_v2_pokemontypes[0].pokemon_v2_type.name
                )
              "
              alt="Secret Type"
            />
          </div>
          <div class="chosen-type-hidden"></div>
        </div>
        <div>
          <div
            class="chosen-type-show fadeIn-swirl-grow"
            v-if="
              chosenPoke.pokemon_v2_pokemontypes &&
              chosenPoke.pokemon_v2_pokemontypes.length == 2 &&
              type2
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
          <div
            v-if="
              chosenPoke.pokemon_v2_pokemontypes &&
              chosenPoke.pokemon_v2_pokemontypes.length == 2
            "
            class="chosen-type-hidden"
          ></div>
        </div>
      </div>
      <div class="chosen-stats-field chosen-gen">
        <div>
          <p>GEN</p>
        </div>
        <div v-if="findGen()" class="chosen-gen-number fadeIn-grow">
          <p>
            {{ chosenPoke.pokemon_v2_pokemonspecy.generation_id }}
          </p>
        </div>
        <div v-else class="chosen-number"></div>
      </div>
      <div class="chosen-stats-field chosen-weight">
        <div>
          <p>WEIGHT</p>
        </div>
        <div v-if="findWeigth()" class="chosen-weight-number fadeIn-grow">
          <p>{{ chosenPoke.weight / 10 }}kg</p>
        </div>
        <div v-else class="chosen-number"></div>
      </div>
      <div class="chosen-stats-field chosen-height">
        <div>
          <p>HEIGHT</p>
        </div>
        <div v-if="findHeight()" class="chosen-height-number fadeIn-grow">
          <p>{{ chosenPoke.height / 10 }}m</p>
        </div>
        <div v-else class="chosen-number"></div>
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
    'pokeFound',
  ],
  data() {
    return {
      type1: false,
      type2: false,
    };
  },
  watch: {
    clickedPokemon: {
      deep: true,
      handler() {
        this.findType();
        this.findGen();
      },
    },
  },
  methods: {
    findType() {
      let clicked = this.clickedPokemon
        .map((p) =>
          p.pokemon_v2_pokemontypes.map((p) => p.pokemon_v2_type.name)
        )
        .flat();
      let chosen = this.chosenPoke.pokemon_v2_pokemontypes.map(
        (p) => p.pokemon_v2_type.name
      );
      if (chosen.length > 1 && clicked.includes(chosen[1])) {
        this.type2 = true;
      }
      if (clicked.includes(chosen[0])) {
        this.type1 = true;
      }
    },
    findGen() {
      let clicked = this.clickedPokemon.map(
        (p) => p.pokemon_v2_pokemonspecy.generation_id
      );
      let chosen = this.chosenPoke.pokemon_v2_pokemonspecy.generation_id;
      if (clicked.includes(chosen)) {
        return true;
      } else {
        return false;
      }
    },
    findWeigth() {
      let clicked = this.clickedPokemon.map((p) => p.weight);
      let chosen = this.chosenPoke.weight;
      if (clicked.includes(chosen)) {
        return true;
      } else {
        return false;
      }
    },
    findHeight() {
      let clicked = this.clickedPokemon.map((p) => p.height);
      let chosen = this.chosenPoke.height;
      if (clicked.includes(chosen)) {
        return true;
      } else {
        return false;
      }
    },
  },
};
</script>

<style lang="sass">
@import '../variables.sass'

.chosen-box
  height: fit-content
  margin: 20px
  width: 450px
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
    text-shadow: 2px 2px 2px $shadow
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
  .chosen-type-show
    height: 30px
    width: 30px
    margin: 0 10px
    position: absolute
    img
      height: 30px
      width: 30px

  .chosen-type-hidden
    height: 30px
    width: 30px
    margin: 0 10px
    background: $shadow
    border-radius: 50%

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
  display: flex
  flex-direction: column
  align-items: center
  justify-content: center

.chosen-weight
  width: 85px
  display: flex
  flex-direction: column
  align-items: center
  justify-content: center


.chosen-height
  width: 85px
  display: flex
  flex-direction: column
  align-items: center
  justify-content: center

.chosen-number
  height: 20px
</style>
