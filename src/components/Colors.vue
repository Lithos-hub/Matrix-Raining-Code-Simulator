<template>
  <div>
    <div id="interface">
      <div class="row">
        <div class="column">
          <button
            class="color-btn"
            id="red-btn"
            @click="changeColor('redChars')"
          ></button>
        </div>
        <div class="column">
          <button
            class="color-btn"
            id="blue-btn"
            @click="changeColor('blueChars')"
          ></button>
        </div>
        <div class="column">
          <button
            class="color-btn"
            id="yellow-btn"
            @click="changeColor('yellowChars')"
          ></button>
        </div>
        <div class="column">
          <button
            class="color-btn"
            id="orange-btn"
            @click="changeColor('orangeChars')"
          ></button>
        </div>
        <div class="column">
          <button
            class="color-btn"
            id="cyan-btn"
            @click="changeColor('cyanChars')"
          ></button>
        </div>
        <div class="column">
          <button
            class="color-btn"
            id="white-btn"
            @click="changeColor('whiteChars')"
          ></button>
        </div>
        <div class="column">
          <button
            class="color-btn"
            id="green-btn"
            @click="changeColor('greenChars')"
          ></button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  methods: {
    closeMenu() {
      this.$emit("close-music-menu");
    },
    changeColor(color) {
      this.$emit("change-color", color);
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../assets/scss/variables.scss";

.row {
  position: absolute;
  left: 0.5%;
  top: 0;
  bottom: 0;
  justify-content: center;
  align-items: center;
  width: 100%;
  justify-self: center;
  display: flex;
}

.color-btn {
  padding: 3em;
  margin: 0 auto;
  border: 0;
  border-radius: 15px;
  cursor: pointer;
  box-shadow: 0px 5px 5px black;
  border: 2px solid #303030;
  transition: all 0.5s ease;

  &:hover {
    filter: brightness(1.5);
  }
}

#red-btn {
  background: linear-gradient(120deg, red, rgb(79, 0, 0));
}
#blue-btn {
  background: linear-gradient(120deg, blue, rgb(0, 0, 60));
}
#yellow-btn {
  background: linear-gradient(120deg, yellow, rgb(61, 61, 0));
}
#orange-btn {
  background: linear-gradient(120deg, orange, rgb(58, 38, 0));
}
#cyan-btn {
  background: linear-gradient(120deg, cyan, rgb(0, 79, 79));
}
#white-btn {
  background: linear-gradient(120deg, white, rgb(94, 94, 94));
}
#green-btn {
  background: linear-gradient(120deg, $green, rgb(0, 90, 8));
}
</style>
