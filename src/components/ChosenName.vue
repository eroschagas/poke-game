<template>
  <div class="chosen-name">
    <h1>{{ title }}</h1>
  </div>
</template>

<script>
export default {
  name: 'ChosenName',
  props: ['name', 'pokeFound'],
  data() {
    return {
      title: 'Who is that pokémon?',
    };
  },
  watch: {
    pokeFound() {
      if (this.pokeFound) {
        this.writeText('it is...');
        setTimeout(() => {
          this.writeText(this.name);
        }, 4000);
      } else {
        this.writeText('Who is that pokémon?');
      }
    },
  },
  created() {
    this.writeText('Who is that pokémon?');
  },
  methods: {
    writeText(text) {
      this.title = '';
      let index = 0;
      let write = () => {
        if (index < text.length) {
          this.title += text.charAt(index);
          index += 1;
          setTimeout(() => {
            write();
          }, 100);
        }
      };
      write();
    },
  },
};
</script>

<style lang="sass">
.chosen-name
  display: flex
  justify-content: center
  align-items: center
  text-align: center
  height: 82px
  width: 300px
</style>
