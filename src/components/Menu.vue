<template>
  <div id="menu">
    <h3 class="menu-title text-center fadeIn">Options</h3>
    <div class="menu-list fadeIn">
      <p class="menu-item text-center fadeIn" @click="showMusicMenu">
        Play Music
      </p>
      <p class="menu-item text-center fadeIn">Change colors</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  methods: {
    showMusicMenu() {
      this.$emit("show-music-menu");
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../assets/scss/variables.scss";
@import "../assets/scss/app.scss";

#menu {
  font-family: "Consolas";
  opacity: 1;
  p {
    font-family: "Consolas";
    color: white;
    font-size: 15px;
  }
}

.menu-list {
  display: flex;
  margin: 0 auto;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.menu-item {
  cursor: pointer;
  color: white;
  font-family: "Consolas";
  border-bottom: 1px solid white;
  padding: 20px;

  &:hover {
    color: $green !important;
    background: rgb(65, 57, 57);
  }
}

.menu-title {
  color: white;
  font-family: "Consolas";
  letter-spacing: 10px;
}

.fadeIn {
  animation: fadeIn 1s ease-out;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
