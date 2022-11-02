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
  props: ['speed', 'size'],
  data() {
    return {
      x: 0,
    };
  },
  mounted() {
    let loading = this.$refs.loading;
    this.x = 1;
    loading.style.height = this.size + 'px';
    loading.style.width = this.size + 'px';
  },
  watch: {
    x() {
      setTimeout(() => {
        this.rotate(this.x);
        this.x += 45;
      }, this.delay);
    },
  },
  computed: {
    delay() {
      if (this.speed == 1) {
        return 150;
      }
      if (this.speed == 2) {
        return 100;
      }
      if (this.speed == 3) {
        return 50;
      }
      return 100;
    },
  },
  methods: {
    rotate(x) {
      let loading = this.$refs.loading;
      if (loading) {
        loading.style.rotate = x + 'deg';
      }
    },
    beforeDestroy() {
      this.x = 0;
    },
  },
};
</script>
