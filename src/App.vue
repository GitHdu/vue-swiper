<template>
  <div id="app">
    <swiper v-model="selected"
            autoplay>
      <swiper-item name="box1">
        <div class="content"
             style="background: red">content1</div>
      </swiper-item>
      <swiper-item name="box2">
        <div class="content"
             style="background: green">content2</div>
      </swiper-item>
      <swiper-item name="box3">
        <div class="content"
             style="background: yellow">content3</div>
      </swiper-item>
    </swiper>
  </div>
</template>

<script>
import Swiper from './components/Swiper.vue'
import SwiperItem from './components/SwiperItem.vue'

export default {
  name: 'app',
  components: {
    Swiper,
    SwiperItem
  },
  data () {
    return {
      selected: 'box2'
    }
  },
  mounted () {

  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
.content {
  width: 300px;
  height: 150px;
}
</style>
