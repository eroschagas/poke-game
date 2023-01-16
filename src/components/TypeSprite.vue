<template>
  <div class="sprite" v-for="(item, index) in items" :key="index">
    <img :src="typeSprite(item)" :alt="item" />
  </div>
  <!-- <div>
    {{ items }}
  </div> -->
</template>

<script>
export default {
  name: 'TypeSprite',
  props: ['pokemon', 'typeSprite'],
  computed: {
    items() {
      let data = [];
      this.pokemon.pokemon_v2_pokemontypes.map((p) =>
        data.push(p.pokemon_v2_type.name)
      );
      return data;
    },
  },
};
</script>

<style lang="scss" scoped>
.sprite {
  display: flex;
  height: 100%;
  width: 100%;
  padding: 2px;
  img {
    height: 100%;
    width: 100%;
    object-fit: contain;
  }
}
</style>
