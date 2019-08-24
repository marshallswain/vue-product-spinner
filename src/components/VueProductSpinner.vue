<template>
  <picture ref="componentContainer" class="vue-product-spinner">
    <template v-if="imagesPreloaded">
      <img
        draggable="false"
        tabindex="1"
        :src="spinner.currentPath"
        @keydown="handleKeydown"
        @mouseup.stop="handleMouseUp"
        @mousedown.stop="handleMouseDown"
        @mousemove.stop="handleMouseMove"
        @touchstart.stop="handleTouchStart"
        @touchend="handleTouchEnd"
        @touchmove="handleTouchMove"
      />
      <input
        v-if="slider"
        type="range"
        tabindex="1"
        min="1"
        :max="spinner.size"
        :value="spinner.current"
        class="vue-product-spinner-slider"
        :class="sliderClass"
        @input="handleSlider"
      />
    </template>
    <slot v-else>
      Loading images...
    </slot>
  </picture>
</template>

<script>
import PreloadImages from './PreloadImages.js'

export default {
  name: 'VueProductSpinner',

  props: {
    images: {
      type: Array,
      required: true
    },
    infinite: {
      type: Boolean,
      default: true
    },
    speed: {
      type: Number,
      default: 3
    },
    touchDrag: {
      type: Boolean,
      default: true
    },
    mouseWheel: {
      type: Boolean,
      default: true
    },
    mouseDrag: {
      type: Boolean,
      default: true
    },
    slider: {
      type: Boolean,
      default: false
    },
    sliderClass: {
      type: String,
      default: ''
    }
  },

  data() {
    return {
      imagesPreloaded: false,
      speedController: 0,
      spinner: {
        current: 0,
        size: 0,
        currentPath: null
      },
      mouse: {
        isMoving: false
      },
      touch: {
        isMoving: false,
        initialX: 0,
        currentX: 0
      }
    }
  },

  beforeMount() {
    PreloadImages(this.images).then(() => (this.imagesPreloaded = true))
  },

  mounted() {
    this.$refs.componentContainer &&
      this.$refs.componentContainer.addEventListener(
        'wheel',
        this.handleWheel,
        false
      )
  },

  destroyed() {
    this.$refs.componentContainer &&
      this.$refs.componentContainer.removeEventListener(
        'wheel',
        this.handleWheel
      )
  },

  created() {
    this.spinner.size = this.images.length
    this.spinner.currentPath = this.images[0]
  },

  methods: {
    handleKeydown(event) {
      if (event.keyCode === 39) {
        event.preventDefault()
        this.handleMovement(1, true)
      }
      if (event.keyCode === 37) {
        event.preventDefault()
        this.handleMovement(-1, true)
      }
    },

    handleSlider(event) {
      this.spinner.current = parseInt(event.target.value)
      this.spinner.currentPath = this.images[event.target.value - 1]
    },

    handleMouseDown() {
      this.mouse.isMoving = true
    },

    handleMouseUp() {
      this.mouse.isMoving = false
    },

    handleMouseMove(event) {
      if (
        this.mouse.isMoving &&
        this.mouseDrag &&
        Math.abs(event.movementX) > 1
      ) {
        this.handleMovement(event.movementX)
      }
    },

    handleTouchStart(event) {
      event.preventDefault()
      this.touch.isMoving = true
      this.touch.initialX = event.touches[0].pageX
      this.touch.currentX = event.touches[0].pageX
    },

    handleTouchEnd() {
      this.touch.isMoving = false
    },

    handleTouchMove(event) {
      if (this.touchDrag) {
        const delta = -(this.touch.currentX - event.touches[0].pageX)
        this.touch.currentX = event.touches[0].pageX
        if (Math.abs(delta) > 1.2) {
          this.handleMovement(delta)
        }
      }
    },

    handleWheel(event) {
      if (this.mouseWheel && Math.abs(event.deltaY) > 1) {
        event.preventDefault()
        this.handleMovement(event.deltaY)
      }
    },

    handleMovement(delta, force = false) {
      this.speedController++
      if (this.speedController < this.speed && !force) {
        return
      }

      if (this.speedController > this.speed && !force) {
        this.speedController = 0
      }

      if (delta >= 0) {
        /**
         * User is moving forward
         */
        if (
          this.spinner.current >= 0 &&
          this.spinner.current < this.spinner.size
        ) {
          this.spinner.current++
          this.spinner.currentPath = this.images[this.spinner.current - 1]
        } else {
          if (this.infinite) {
            this.spinner.current = 1
            this.spinner.currentPath = this.images[this.spinner.current - 1]
          }
        }
      } else {
        /**
         * User is moving backward
         */
        if (this.spinner.current >= 0 && this.spinner.current - 1 > 0) {
          this.spinner.current--
          this.spinner.currentPath = this.images[this.spinner.current - 1]
        } else {
          if (this.infinite) {
            this.spinner.current = this.spinner.size
            this.spinner.currentPath = this.images[this.spinner.current - 1]
          }
        }
      }
    }
  }
}
</script>
