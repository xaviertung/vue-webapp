<template>
  <img alt="Vue logo" ref="img" src="../assets/loading.gif">
</template>

<script>

export default {
  name: 'my-img',
  props: {
    src: String,
  },
  mounted() {
    const { img } = this.$refs;
    const realImage = new Image();
    realImage.src = this.src;
    realImage.onload = () => {
      setTimeout(() => {
        img.src = this.src;
      }, 1000);
    };
  },
};
</script>

<style scoped lang="less">

</style>
