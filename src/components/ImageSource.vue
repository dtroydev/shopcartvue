<template>
  <div>
    <transition name="fade">
      <img :src="image" @load="onLoaded" v-show="loaded" />
    </transition>
  </div>
</template>
<script>
export default {
  name: 'ImageSource',
  props: ['product', 'baseUrl'],
  data() {
    return {
      loaded: false,
    };
  },
  methods: {
    onLoaded() {
      this.loaded = true;
    },
  },
  computed: {
    image() {
      this.loaded = false;
      const url = `${this.baseUrl}${this.product.product},${this.product.varieties[this.product.selectedVariety].color}/all`;
      this.$emit('loader', url);
      return url;
    },
  },
};
</script>
<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 3s ease-in-out;
}

.fade-leave,
.fade-enter-to {
  opacity: 1;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
