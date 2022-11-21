<template>
  <div class="chosen-stats-field-2">
    <div class="chosen-stats-title">
      <p>{{ statName }}</p>
    </div>
    <div v-if="found" :class="[chosen ? 'fadeIn-grow' : '']">
      <p>{{ stat }}</p>
    </div>
    <div v-if="!chosen" class="fadeIn-grow compare">
      <ion-icon :name="icon"></ion-icon>
    </div>
    <p v-if="found" class="chosen-stats-unit fade-in">{{ unit }}</p>
  </div>
</template>

<script>
export default {
  name: 'PokeStat',
  props: ['statName', 'found', 'stat', 'unit', 'chosenStat', 'chosen'],
  computed: {
    icon() {
      if (this.chosenStat > this.stat) {
        return 'arrow-up-circle-outline';
      }
      if (this.chosenStat < this.stat) {
        return 'arrow-down-circle-outline';
      }
      return 'checkmark-circle-outline';
    },
  },
};
</script>

<style lang="sass">
@import '../variables.sass'

.chosen-stats-field-2
  background: $chosenBackgroundFields
  display: flex
  flex-direction: column
  justify-content: center
  align-items: center
  height: 50px
  width: 50px
  // box-shadow: 2px 2px 2px $shadow
  margin: 10px 10px 5px 0
  white-space: nowrap
  border-radius: 50%
  font-family: $pixel
  font-weight: normal
  position: relative
  // text-shadow: 2px 2px 2px $shadow

.chosen-stats-title
  position: absolute
  top: -5px
  background: $chosenBackgroundFields
  border-radius: 5px
  padding: 0 3px
  z-index: 10

.chosen-stats-unit
  position: absolute
  bottom: 3px
  font-size: 9px
  text-rendering: geometricPrecision
  z-index: 10

.compare
  position: absolute
  left: -5px
  bottom: -3px
  background: $chosenBackgroundFields
  height: 20px
  width: 20px
  display: flex
  justify-content: center
  align-items: center
  font-size: 20px
  border-radius: 50%
</style>
