<template>
  <div id="app" @click="closeMenu">
    <ChangeLog v-if="changeLog" @close-changelog="closeChangelog" />
    <PrimaryCode
      class="code-component"
      :custom-char-class="charClass"
      v-for="(num, i) in numOfCodeComponents"
      :key="i + 'A'"
      :code-index="i"
    />
    <SecondaryCode
      class="code-component-2"
      :custom-char-class="charClass"
      v-for="(num, i) in numOfCodeComponents_2"
      :key="i + 'B'"
      :code-index="i"
    />
    <div id="version" @click="changeLog = !changeLog">
      {{ version }}
    </div>
    <div id="menu" @click="openMenu">
      <p id="menu-text">MENU</p>
    </div>
    <Menu
      id="menu-component"
      v-if="menu"
      class="expanded-menu"
      @show-music-menu="showMusicMenu"
      @show-colors-menu="showColorsMenu"
    />
    <Music
      id="music-component"
      v-if="onPlayMusic"
      class="music-player"
      @close-music-menu="closeMusicMenu"
    />
    <Colors
      id="colors-component"
      v-if="onChangeColors"
      class="colors-menu"
      @close-colors-menu="closeColorsMenu"
      @change-color="changeColor"
    />
  </div>
</template>

<script>
import ChangeLog from "@/components/Changelog.vue";
import SecondaryCode from "@/components/SecondaryCode.vue";
import PrimaryCode from "@/components/PrimaryCode.vue";
import Music from "@/components/Music.vue";
import Menu from "@/components/Menu.vue";
import Colors from "@/components/Colors.vue";
export default {
  components: {
    PrimaryCode,
    SecondaryCode,
    ChangeLog,
    Music,
    Menu,
    Colors,
  },
  name: "App",
  data() {
    return {
      menu: false,
      onPlayMusic: false,
      onChangeColors: false,
      major: 1,
      minor: 0,
      patch: 0,
      changeLog: false,
      numOfCodeComponents: 30,
      numOfCodeComponents_2: 30,
      charClass: "greenChars",
    };
  },
  computed: {
    version() {
      return `V${this.major}.${this.minor}.${this.patch}`;
    },
  },
  methods: {
    closeChangelog() {
      this.changeLog = false;
    },
    openMenu() {
      this.menu = true;
    },
    closeMenu() {
      let menu = document.querySelector("#menu");
      let colorsMenu = document.querySelector("#colors-component");
      window.addEventListener("click", (e) => {
        if (menu !== null && !menu.contains(e.target)) {
          this.menu = false;
        }
        if (colorsMenu !== null && !colorsMenu.contains(e.target)) {
          this.closeColorsMenu();
        }
      });
    },
    showMusicMenu() {
      this.onPlayMusic = true;
      setTimeout(() => {
        let musicComponent = document.querySelector("#music-component");
        musicComponent.style.transition = "0.3s ease-out";
        musicComponent.style.bottom = "0px";
      }, 100);
    },
    closeMusicMenu() {
      let musicComponent = document.querySelector("#music-component");
      musicComponent.style.transition = "0.3s ease-out";
      musicComponent.style.bottom = "-100px";
      setTimeout(() => {
        this.onPlayMusic = false;
      }, 1000);
    },
    showColorsMenu() {
      this.onChangeColors = true;
      setTimeout(() => {
        let musicComponent = document.querySelector("#colors-component");
        musicComponent.style.transition = "0.3s ease-out";
        musicComponent.style.bottom = "0px";
      }, 100);
    },
    closeColorsMenu() {
      let musicComponent = document.querySelector("#colors-component");
      musicComponent.style.transition = "0.3s ease-out";
      musicComponent.style.bottom = "-100px";
      setTimeout(() => {
        this.onChangeColors = true;
      }, 1000);
    },
    changeColor(customClass) {
      this.charClass = customClass;
    },
  },
};
</script>

<style lang="scss" scoped>
@import "./assets/scss/variables.scss";
@import "./assets/scss/app.scss";
@font-face {
  font-family: "Matrix";
  src: url("./assets/fonts/matrix.ttf") format("truetype");
}

* {
  font-family: "Matrix";
}

#app {
  position: fixed;
  filter: contrast(1.5);
  filter: brightness(1.5);
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background-color: #050505;
}

#version {
  cursor: pointer;
  font-family: "Consolas";
  font-size: 12px;
  color: $green;
  position: fixed;
  z-index: 3;
  bottom: 0;
  left: 0;
  background: #151515;
  padding: 10px;
  opacity: 1;
  transition: 0.3s ease-in-out;

  &:hover {
    background: white;
    color: black;
    transition: 0.3s ease-in-out;
  }
}
.menu-icon {
  opacity: 0;
}

.code-component-2 {
  opacity: 0.5;
}

.expanded-menu {
  position: fixed;
  bottom: 0;
  right: 0;
  background: #151515;
  box-shadow: 0px 0px 10px white, 0px 0px 20px $green;
  height: 200px;
  width: 500px;
  z-index: 2;
  transition: all 0.1s ease-out;
  animation: expand 0.1s ease-in;
}

#music-component {
  border-inline: 3px solid #1f8615;
  border-top: 2px solid $green;
  width: 500px;
  height: 45px;
  background: linear-gradient(120deg, #303030, #151515);
  position: fixed;
  bottom: -100px;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 2;
}

#colors-component {
  border-top-left-radius: 15px;
  border-top-right-radius: 15px;
  width: 550px;
  height: 2em;
  position: fixed;
  bottom: -100px;
  left: 50%;
  background: linear-gradient(120deg, #303030, #151515);
  border-left: 2px solid #7b7b7b;
  border-right: 2px solid #7b7b7b;
  transform: translate(-50%, 0);
  z-index: 9999;
  padding: 2em;
}

#menu {
  position: absolute;
  right: 0;
  bottom: 0;
  width: clamp(20px, 50%, 75px);
  height: clamp(20px, 50%, 75px);
  border: 1px solid $green;
  box-shadow: inset 0px 0px 5px black;
  cursor: pointer;
  background: transparent;
  z-index: 1;
  transition: 0.2s;
  clip-path: polygon(100% 0, 100% 0%, 100% 100%, 0% 100%);
  &:hover {
    #menu-text {
      color: $green;
    }
  }
}

#menu-text {
  transition: all 0.3s ease-out;
  position: fixed;
  font-size: clamp(18px, 75%, 30px);
  right: 20px;
  text-align: center;
  bottom: 0px;
  color: rgb(255, 255, 255);
  transform: rotate(-45deg);
}

@keyframes expand {
  0% {
    height: 50px;
    width: 50px;
  }
  100% {
    height: 200px !important;
    width: 500px !important;
  }
}
</style>
