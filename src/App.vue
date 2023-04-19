<template>
  <div class="wrapper">
    <div class="slider">
      <a class="prev" @click="prev" href="#">&#10094;</a>
      <img class="slider__image" :src="image" />
      <a class="next" @click="next" href="#">&#10095;</a>
    </div>
    <ol class="vue-carousel__indicators">
      <li class="vue-carousel__indicator" v-bind:class="{ active: index === ind - 1 }" @click="nextTo(ind - 1)" v-for="ind in images.length"></li>
    </ol>
  </div>
</template>
<script>
export default {
  name: "App",
  data() {
    const images = [
      'https://a.d-cd.net/1c46386s-960.jpg',
      'https://a.d-cd.net/fc46386s-960.jpg',
      'https://a.d-cd.net/a246386s-960.jpg',
      'https://topruscar.ru/assets/images/terms/term2808_001_1.jpg',
    ];
    return { index: 0, image: images[0], images: images };
  },
  methods: {
    next() {
      clearInterval(this.timer);
      this.autoChangeSlide();
      this.index = (this.index + 1) % this.images.length;
      this.image = this.images[this.index];
    },
    nextTo(index) {
      clearInterval(this.timer);
      this.autoChangeSlide();
      this.index = index;
      this.image = this.images[this.index];
    },
    prev() {
      clearInterval(this.timer);
      this.autoChangeSlide();
      this.index = (this.index - 1) % this.images.length;
      if (this.index === -1) {
        this.index = this.images.length - 1;
      }
      this.image = this.images[this.index];
    },
    autoChangeSlide() {
      this.timer = setInterval(() => {
        this.index = (this.index + 1) % this.images.length;
        this.image = this.images[this.index];
      }, 5000);
    },
  },
  beforeMount() {
    this.autoChangeSlide();
  },
};
</script>

<style>
.prev, .next {
  background-color: rgba(0,0,0,0.9);
  cursor: pointer;
  padding: 16px;
  color: white;
  font-weight: bold;
  font-size: 18px;
  transition: 0.7s ease;
  border-radius: 0 4px 4px 0;
  text-decoration: none;
  user-select: none;
}
/* Position the "next button" to the right */
/* On hover, add a black background color with a little bit see-through */

.vue-carousel__indicators {
  display: flex;
  justify-content: center;
  list-style: none;
  margin: 0;
  padding: 0;
  position: absolute;
  transform: translateX(-50%);
  z-index: 1;
}

.vue-carousel__indicator {
  cursor: pointer;
  height: 1.5rem;
  margin: 0.125rem;
  opacity: .5;
  position: relative;
  transition: opacity .15s;
  width: 1.5rem;
}
.vue-carousel__indicator::before {
  border-radius: 0.6225rem;
  border: #000 0.2rem solid;
  content: "";
  display: block;
  height: 1.25rem;
  width: 1.25rem;
  left: 50%;
  position: absolute;
  top: 50%;
  transform: translate(-50%, -50%);
}

.vue-carousel__indicator.active {
  opacity: 1;
}
.wrapper {
  display: flex;
  justify-content: center;
}
.slider {
  display: flex;
}
img {
  width: 960px;
}
</style>