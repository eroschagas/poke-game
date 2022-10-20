<template>
  <div ref="teste" class="pick-section">
    <div v-for="(item, index) in clickedPokemon" :key="index">
      <div
        :id="item.id"
        :ref="'card-' + item.id"
        class="pick-card fadeIn-slideLeft"
      >
        <div class="pick-value pick-name">{{ item.name }}</div>
        <div class="pick-sprite">
          <img
            :src="pokeSprite(item.id - 1, 'other', 'official-artwork')"
            :alt="item.name"
          />
        </div>
        <div class="pick-value">
          <p>{{ item.weight / 10 }}KG</p>
        </div>
        <div class="pick-value">
          <p>{{ item.height / 10 }}M</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PokePick',
  props: ['clickedPokemon', 'pokeSprite'],
  computed: {
    listMin() {
      if (this.clickedPokemon.length >= 4) {
        return this.clickedPokemon.length - 4;
      }
      return 0;
    },
    listMax() {
      return this.clickedPokemon.length;
    },
  },
  watch: {
    clickedPokemon: {
      deep: true,
      handler() {
        this.$nextTick(() => {
          var lastCardHeight =
            this.$refs[
              'card-' + this.clickedPokemon[this.clickedPokemon.length - 1].id
            ][0].offsetHeight;
          this.$refs.teste.scrollTo(0, this.clickedPokemon.length * lastCardHeight);
        });
      },
    },
  },
};
</script>

<style lang="sass">
@import '../variables.sass'

.pick-section
  height: 256px
  overflow-y: hidden
  overflow-x: hidden
  scroll-behavior: smooth
  margin: 20px

.pick-card
  display: flex
  justify-content: flex-start
  align-items: center

.pick-sprite
  margin: 10px
  img
    height: 40px
    width: 40px
.pick-value
  margin: 10px
  width: 60px
  p
    text-align: center
    font-family: $pixel

.pick-name
  width: 130px
  font-family: $pixel
</style>
