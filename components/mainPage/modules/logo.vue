<template>
    <div class="img-container">
        <img
          src="https://michaelshof-sammatz.de/blog/wp-content/uploads/2020/11/logo.png"
        />
      </div>
</template>
<script>
    export default {
        name: 'Logo'
    }
</script>
<style scoped lang="scss">
.img-container {
    position: absolute;
    bottom: 12px;
    left: 50%;
    transform: translateX(-50%);
    img {
      width: 280px;
    }
  }
</style>