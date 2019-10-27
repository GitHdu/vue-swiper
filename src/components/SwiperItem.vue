<template>
  <transition>
    <div class="swiper-item"
         :class="{reverse}"
         v-if="isShow">
      <slot></slot>
    </div>
  </transition>
</template>

<script>
export default {
  props: {
    name: {
      type: String,
      required: true
    }
  },
  computed: {
    isShow () {
      return this.selected === this.name
    }
  },
  data () {
    return {
      selected: '',
      reverse: ''
    }
  }
}
</script>

<style scoped>
.v-enter-active,
.v-leave-active {
  transition: all.5s;
}
.v-leave-to {
  transform: translateX(-100%);
}
.v-enter {
  transform: translateX(100%);
}

.v-leave-to.reverse {
  transform: translateX(100%);
}
.v-enter.reverse {
  transform: translateX(-100%);
}
.v-enter-active {
  position: absolute;
  top: 0;
  left: 0;
}
</style>
