<template>
  <div class="general-map">
    <img src="michaelshof_map.jpg" alt="" />
    <div class="current-location">
      <div class="point-location"></div>
    </div>
  </div>
</template>
<script>
export default {
    name: 'Map', 
    mounted(){
        this.eventHandler()
    },
  methods: {
    eventHandler() {
      /**
       * On click map
       */
      const map = document.querySelector(".general-map");
      map.addEventListener("click", function () {
        map.classList.toggle("full-map");
      });
    },
  },
};
</script>
<style lang="scss" scoped>
$size-map: 124px;
.general-map {
  position: absolute;
  bottom: 16px;
  right: 16px;
  background-color: transparent;
  height: $size-map;
  width: $size-map;
  border-radius: 50%;
  border: 2px solid white;
  transition: 0.3s ease;
  overflow: hidden;
  opacity: 0.6;

  &:hover {
    opacity: 1;
    height: $size-map * 1.3;
    width: $size-map * 1.3;
    cursor: pointer;
  }

  &.full-map {
    height: 100vh;
    width: inherit;
    position: absolute;
    bottom: 0;
    right: 0;
    border: none;
    border-left: 2px solid white;
    border-radius: 0;
    z-index: 9;
    opacity: 1;
    // overflow-y: auto;

    img {
      height: 100%;
    }
    .current-location{
      background-color: darkviolet;
      height: 24px;
      width: 24px;
      position: absolute;
      top: 140px;
      left: 100px;
      clip-path: polygon(0% 15%, 15% 15%, 15% 0%, 85% 0%, 85% 15%, 100% 15%, 100% 85%, 85% 85%, 85% 100%, 15% 100%, 15% 85%, 0% 85%);
      .point-location{
        background-color: white;
        height: 8px;
        width: 8px;
        border-radius: 50%;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
      }
    }
  }

  img {
    width: 100%;
    object-fit: cover;
  }
}
</style>