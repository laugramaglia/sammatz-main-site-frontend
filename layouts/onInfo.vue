<template>
  <div>
    <Nuxt />
    <div class="menu-container">
      <a-space direction="vertical" size="middle">
        <a-tooltip placement="right">
          <template slot="title">
            <span>prompt text</span>
          </template>
          <NuxtLink to="/"
            ><a-icon type="yuque" class="edited-icons"
          /></NuxtLink>
        </a-tooltip>

        <a-tooltip placement="right">
          <template slot="title">
            <span>prompt text</span>
          </template>
          <NuxtLink to="/"
            ><a-icon type="yuque" class="edited-icons"
          /></NuxtLink>
        </a-tooltip>

        <a-tooltip placement="right">
          <template slot="title">
            <span>prompt text</span>
          </template>
          <NuxtLink to="/"
            ><a-icon type="yuque" class="edited-icons"
          /></NuxtLink>
        </a-tooltip>

        <a-tooltip placement="right">
          <template slot="title">
            <span>prompt text</span>
          </template>
          <NuxtLink to="/"
            ><a-icon type="yuque" class="edited-icons"
          /></NuxtLink>
        </a-tooltip>

        <a-tooltip placement="right">
          <template slot="title">
            <span>prompt text</span>
          </template>
          <NuxtLink to="/"
            ><a-icon type="yuque" class="edited-icons"
          /></NuxtLink>
        </a-tooltip>

        <a-tooltip placement="right">
          <template slot="title">
            <span>prompt text</span>
          </template>
          <NuxtLink to="/"
            ><a-icon type="yuque" class="edited-icons"
          /></NuxtLink>
        </a-tooltip>
      </a-space>
    </div>
  <Logo />
  </div>
</template>
<script>
import Logo from '../components/mainPage/modules/logo.vue'
export default {
  components:{
    Logo
  },
  data() {
    return {};
  },
};
</script>

<style lang="scss" scoped>
.menu-container {
  // width: 280px;
  position: fixed;
  top: 50%;
  transform: translateY(-50%);
  left: 0;
  // background-image: url("static/Artboard.svg");
  // background-repeat: no-repeat;
  // background-size: cover;

  .edited-icons {
    padding: 8px;
    font-size: 28px;
    color: violet;
  }
}
</style>