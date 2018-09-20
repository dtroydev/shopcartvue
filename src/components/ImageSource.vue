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
      imgSrc: '',
      loaded: false,
      imgCache: [],
    };
  },
  methods: {
    onLoaded() {
      // console.log('onLoaded called with', e.target);
      this.loaded = true;
    },
  },
  computed: {
    image() {
      // console.log('start computed');
      this.loaded = false;

      const url = `${this.baseUrl}${this.product.product},${this.product.varieties[this.product.selectedVariety].color}/all`;

      if (this.imgCache.includes(url) === false) {
        this.imgCache.push(url);
        // console.log('instant imgsrc');
        this.imgSrc = url;
      } else {
        setTimeout(() => {
          // delay browser cached images to allow the fade transition to show
          // console.log('delayed imgsrc');
          this.imgSrc = url;
        }, 1500);
      }
      // console.log('about to return');
      return this.imgSrc;
    },
  },
};
</script>
<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 1.5s ease-in-out;
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
