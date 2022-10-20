<template>
  <div>
    <img
      ref="loading"
      class="loading"
      src="../assets/loading-icon.svg"
      alt="loading"
    />
  </div>
</template>

<script>
export default {
  name: 'LoadingIcon',
  data() {
    return {
      x: 0,
    };
  },
  created() {
    this.$nextTick(() => {
      this.x = 1;
    });
  },
  watch: {
    x() {
      setTimeout(() => {
        this.rotate(this.x);
        this.x += 1;
      }, 100);
    },
  },
  methods: {
    rotate(x) {
      this.$refs.loading.style.rotate = x * 45 + 'deg';
    },
    beforeDestroy() {
      this.x = 0;
    },
  },
};
</script>

<style lang="sass">
.loading
  // min-height: 30px
  // min-width: 30px
</style>
