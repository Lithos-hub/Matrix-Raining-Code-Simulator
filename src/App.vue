<template>
  <div id="app">
    <ChangeLog v-if="changeLog" />
    <Code v-for="(item, i) in numOfCodeComponents" :key="i" :code-index="i" />
    <div id="version" @mouseover="changeLog = !changeLog">
      {{ version }}
    </div>
  </div>
</template>

<script>
import ChangeLog from "@/components/Changelog.vue";
import Code from "@/components/Code.vue";
export default {
  components: {
    ChangeLog,
    Code,
  },
  name: "App",
  data() {
    return {
      major: 0,
      minor: 8,
      patch: 0,
      changeLog: false,
      numOfCodeComponents: 40,
    };
  },
  computed: {
    version() {
      return `V${this.major}.${this.minor}.${this.patch}`;
    },
  },
};
</script>

<style lang="scss">
// SCSS variables

$green: #00ff00;
$cyan: #00ffff;

@font-face {
  font-family: "Matrix";
  src: url("./assets/fonts/matrix.ttf") format("truetype");
}

* {
  font-family: "Matrix";
}


#app {
  position: absolute;
  // overflow: hidden;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background-color: #151020;
}

#version {
  cursor: pointer;
  font-family: "Consolas";
  font-size: 18px;
  color: $green;
  position: fixed;
  z-index: 3;
  bottom: 0;
  left: 0;
  background: #151515;
  padding: 10px;
  opacity: 1;
}
</style>
