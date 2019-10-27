<template>
  <div class="swiper"
       @mouseenter="mouseenter"
       @mouseleave="mouseleave">
    <div class="viewport">
      <slot></slot>
    </div>
    <div class="dots">
      <span v-for="(item,index) in len"
            :key="index"
            @click="select(index)"
            :class="{'active': activeIndex===index}">
        {{item}}
      </span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      default: ''
    },
    autoplay: {
      type: Boolean,
      default: true
    },
    reverse: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      currentSelected: '',
      len: 0
    }
  },
  computed: {
    activeIndex () {
      return this.names.indexOf(this.currentSelected)
    }
  },
  methods: {
    showChild () {
      // 从哪个name属性开始
      this.currentSelected = this.value || this.$children[0].name
      this.$children.forEach((vm) => {
        this.$nextTick(() => {
          vm.selected = this.currentSelected
        })

        // 正向或反向
        let reverse = this.prevPosition > this.activeIndex
        vm.reverse = reverse

        if (this.tiemr) {
          if (this.prevPostion === 0 && this.activeIndex === this.len - 1) {
            vm.reverse = true
          }
          if (this.prevPostion === this.len - 1 && this.activeIndex === 0) {
            vm.reverse = false
          }
        }
      })
    },
    mouseenter () {
      clearInterval(this.timer)
      this.timer = null
    },
    mouseleave () {
      if (!this.timer) {
        this.run()
      }
    },
    select (newIndex) {
      this.prevPosition = this.activeIndex

      if (newIndex === -1) {
        newIndex = this.len - 1
      }
      if (newIndex > this.names.length) {
        newIndex = 0
      }
      this.$emit('input', this.names[newIndex])
    },
    run () {
      if (this.autoplay) {
        this.timer = setInterval(() => {
          let index = this.activeIndex
          // +1 从左往右 -1 从右边往左
          let newIndex = !this.reverse ? index + 1 : index - 1
          this.select(newIndex)
        }, 2000)
        this.$once('hook:beforeDestroy', () => {
          clearInterval(this.timer)
        })
      }
    }
  },
  watch: {
    value: {
      handler () {
        this.showChild()
      },
      immediate: true
    }
  },
  mounted () {
    this.names = this.$children.map(child => child.name)
    this.len = this.names.length
    this.run()

    this.prevPosition = this.activeIndex
  }
}
</script>

<style scoped>
.swiper {
  width: 300px;
  margin: 0 auto;
  border: 10px solid purple;
}
.viewport {
  position: relative;
  overflow: hidden;
  height: 150px;
}
.dots {
  display: flex;
  justify-content: center;
}
.dots span {
  width: 30px;
  height: 30px;
  line-height: 30px;
  border-radius: 50%;
  text-align: center;
  margin: 0 10px;
  cursor: pointer;
}
.active {
  border: 1px solid black;
}
</style>
