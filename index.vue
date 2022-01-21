<template>
  <div ref="icon" @mouseenter="mouseenter" @mouseleave="mouseleave" />
</template>

<script>

export default {
  name: 'AnimatedIcon',
  data: () => {
    return {
      active: false,
      y: 0
    }
  },
  props: {
    src: {
      type: String,
      required: true
    },
    steps: {
      type: Number,
      required: true  
    },
    width: {
      type: Number,
      default: 128
    },
    height: {
      type: Number,
    },
  },
  computed: {
    _height() {
      return this.height || this.width
    }
  },
  mounted() {
    Object.assign(this.$refs.icon.style, {
      width: `${this.width}px`,
      height: `${this._height}px`,
      backgroundImage: `url(${this.src})`,
      backgroundSize: `100%`,
      backgroundPositionY: `0px`,
    })
  },
  methods: {
    mouseenter(e) {
      this.active = true
      this.forward(e.target)
    },
    mouseleave(e) {
      this.active = false
      this.backward(e.target)
    },
    forward(target) {
      if (this.y > - this._height * this.steps && this.active) {
        this.y -= this._height
        target.style.backgroundPositionY = `${this.y}px`
        requestAnimationFrame(() => {
          this.forward(target)
        })
      }
    },
    backward(target) {
      if (this.y < 0 && !this.active) {
        this.y += this._height
        target.style.backgroundPositionY = `${this.y}px`
        requestAnimationFrame(() => {
          this.backward(target)
        })
      }
    },
  },
}
</script>