<template>
  <div class="about">
    <h1>This is an about page</h1>
    <div>
      <Calender />
    </div>
    <div>
      <Carousel />
    </div>
  </div>
</template>
<script>
import Carousel from '@/components/Carousel.vue';

const Calender = () => import(/* webpackChunkName: "calender" */ '@/components/Calender.vue');
export default {
  name: 'home',
  components: {
    Calender,
    Carousel,
  },
};
</script>
