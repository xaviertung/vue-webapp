<template>
  <div style="width:100%;overflow: hidden;">
    <h3>轮播图</h3>
    <ul class="list"
      ref="ref1"
      :style="{width: list.length * width + 'px', transform: 'translateX(' + position + 'px)'}"
      @touchstart="handleTouchStart"
      @touchend="handleTouchEnd"
      >
      <li class="list-item"
        :style="{width: width + 'px'}"
        v-for="item in list"
        :key="item">
        <img :src="item" style="width:100%;height:100%;"/>
      </li>
    </ul>
  </div>
</template>
<style scoped>
.list {
  padding-left: none;
  padding-inline-start: 0;
  /* transform: translateX(375px); */
}
.list-item {
  display: inline-block;
  height: 150px;
  box-sizing: border-box;
  padding: 10px;
  background: #8f8f8f;
  transition: transform 2s;
}
</style>
<script>
export default {
  name: 'Carousel',
  data() {
    return {
      width: document.documentElement.clientWidth,
      list: ['http://hanzichi.github.io/2015/css3-picture-carousel/img/img0.png',
        'http://hanzichi.github.io/2015/css3-picture-carousel/img/img1.png',
        'http://hanzichi.github.io/2015/css3-picture-carousel/img/img2.png',
        'http://hanzichi.github.io/2015/css3-picture-carousel/img/img3.png',
        'http://hanzichi.github.io/2015/css3-picture-carousel/img/img4.png',
        'http://hanzichi.github.io/2015/css3-picture-carousel/img/img5.png',
        'http://hanzichi.github.io/2015/css3-picture-carousel/img/img6.png',
        'http://hanzichi.github.io/2015/css3-picture-carousel/img/img7.png',
        'http://hanzichi.github.io/2015/css3-picture-carousel/img/img8.png'],
      position: 0,
      startX: 0,
      endX: 0,
    };
  },
  methods: {
    handleTouchStart(e) {
      this.startX = e.changedTouches[0].clientX;
    },
    handleTouchEnd(e) {
      this.endX = e.changedTouches[0].clientX;
      const disX = this.endX - this.startX;
      console.log(disX);
      console.log('position:', this.position);
      console.log('max position:', -((this.list.length - 1) * this.width));

      if (disX > 0) {
        // -750 -375 0
        if (this.position < 0) {
          this.position += this.width;
        }
        // this.$refs.ref1.style.transform=`translateX(${this.position})`;
        // console.log(this.$refs.ref1.style.transform)
        // console.log(`translateX(${this.position})`)
      }
      if (disX < 0) {
        // 0 -375 -750
        if (this.position > -((this.list.length - 1) * this.width)) this.position -= this.width;
        // this.$refs.ref1.style.transform=`translateX(${this.position})`;
        // console.log(this.$refs.ref1.style.cssText)
        // console.log(`translateX(${this.position})`)
      }
    },
  },
};
</script>
