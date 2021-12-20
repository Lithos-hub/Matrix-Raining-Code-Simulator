<template>
  <div id="menu">
    <h3 class="menu-title text-center fadeIn">Options</h3>
    <div class="menu-list fadeIn">
      <p class="menu-item text-center fadeIn" @click="showMusicMenu">
        Play Music
      </p>
      <p class="menu-item text-center fadeIn" @click="showColorsMenu">
        Change colors
      </p>
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
    showColorsMenu() {
      this.$emit("show-colors-menu");
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
  justify-content: center;
  align-items: center;
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
