<template>
  <div class="carousel">
    <div class="inner" ref="inner" :style="innerStyles">
      <div class="card" v-for="card in cards" :key="card">
        <img :src="card['url']" alt="src/assets/error.png">
        <span>{{ card['title'] }}</span>
      </div>
    </div>
  </div>
  <button @click="prev">назад</button>
  <button @click="next">вперед</button>
</template>

<script>
export default {
  data () {
    return {
      cards: [{url: "https://a.d-cd.net/1c46386s-960.jpg", title: "Plymouth Fury III 2-door Hardtop"},
        {url: "https://a.d-cd.net/fc46386s-960.jpg", title: "Cadillac de Ville"},
        {url: "https://a.d-cd.net/a246386s-960.jpg", title: "Chevrolet Pickup"}],
      innerStyles: {},
      step: '',
      transitioning: false
    }
  },

  mounted () {
    this.setStep()
    this.resetTranslate()
  },

  methods: {
    setStep () {
      const innerWidth = this.$refs.inner.scrollWidth
      const totalCards = this.cards.length
      this.step = `${innerWidth / totalCards}px`
    },

    next () {
      if (this.transitioning) return

      this.transitioning = true

      this.moveLeft()

      this.afterTransition(() => {
        const card = this.cards.shift()
        this.cards.push(card)
        this.resetTranslate()
        this.transitioning = false
      })
    },

    prev () {
      if (this.transitioning) return

      this.transitioning = true

      this.moveRight()

      this.afterTransition(() => {
        const card = this.cards.pop()
        this.cards.unshift(card)
        this.resetTranslate()
        this.transitioning = false
      })
    },

    moveLeft () {
      this.innerStyles = {
        transform: `translateX(-${this.step})
                    translateX(-${this.step})`
      }
    },

    moveRight () {
      this.innerStyles = {
        transform: `translateX(${this.step})
                    translateX(-${this.step})`
      }
    },

    afterTransition (callback) {
      const listener = () => {
        callback()
        this.$refs.inner.removeEventListener('transitionend', listener)
      }
      this.$refs.inner.addEventListener('transitionend', listener)
    },

    resetTranslate () {
      this.innerStyles = {
        transition: 'none',
        transform: `translateX(-${this.step})`
      }
    }
  }
}
</script>

<style>
.carousel {
  width: 480px;
  overflow: hidden;
  position: center;
}

.inner {
  transition: transform 0.6s;
  white-space: nowrap;
}

.card {
  width: 480px;
  margin-right: 10px;
  display: inline-flex;
  flex-direction: column;

  /* optional */
  height: 480px;
  color: gray;
  border-radius: 4px;
  align-items: center;
  justify-content: center;
}

img {
  max-width: 480px;
  max-height: 320px;
}

/* optional */
button {
  margin-right: 5px;
  margin-top: 10px;
}
</style>