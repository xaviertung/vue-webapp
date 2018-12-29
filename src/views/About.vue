<template>
  <div class="about">
    <h1>This is an about page</h1>
    <div>
      <Calender />
    </div>
  </div>
</template>
<script>
import Calender from '@/components/Calender.vue';

export default {
  name: 'home',
  components: {
    Calender,
  },
};
</script>
