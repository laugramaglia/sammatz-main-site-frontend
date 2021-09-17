<template>
  <div>
    <ThreeJs />
    <MenuSide />
    <div class="height"></div>
  </div>
</template>

<script>
import ThreeJs from '../components/mainPage/ThreeJs.vue'
import MenuSide from '../components/mainPage/MenuSide.vue'
export default {
  components:{
    ThreeJs,
    MenuSide
  }
}
</script>

<style lang="scss" scoped>
.height{
  height: 600px;
}
</style>
