<template>
  <div class="chosen-stats-field-2">
    <div class="chosen-stats-title">
      <p>{{ stat }}</p>
    </div>
    <div v-if="found" class="fadeIn-grow">
      <p>{{ chosenStat }}</p>
    </div>
    <p v-if="found" class="chosen-stats-unit fade-in">{{ unit }}</p>
  </div>
</template>

<script>
export default {
  name: 'PokeStat',
  props: ['stat', 'found', 'chosenStat', 'unit'],
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
  margin: 10px 10px 10px 0
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

.chosen-stats-unit
  position: absolute
  bottom: 3px
  font-size: 9px
  text-rendering: geometricPrecision
</style>
