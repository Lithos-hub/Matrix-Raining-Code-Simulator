<template>
  <div id="app" @click="closeMenu">
    <ChangeLog v-if="changeLog" @close-changelog="closeChangelog" />
    <PrimaryCode
      class="code-component"
      v-for="(num, i) in numOfCodeComponents"
      :key="i + 'A'"
      :code-index="i"
    />
    <SecondaryCode
      class="code-component-2"
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
    />
    <Music
      id="music-component"
      v-if="onPlayMusic"
      class="music-player"
      @close-music-menu="closeMusicMenu"
    />
  </div>
</template>

<script>
import ChangeLog from "@/components/Changelog.vue";
import SecondaryCode from "@/components/SecondaryCode.vue";
import PrimaryCode from "@/components/PrimaryCode.vue";
import Music from "@/components/Music.vue";
import Menu from "@/components/Menu.vue";
export default {
  components: {
    PrimaryCode,
    SecondaryCode,
    ChangeLog,
    Music,
    Menu,
  },
  name: "App",
  data() {
    return {
      menu: false,
      onPlayMusic: false,
      major: 0,
      minor: 9,
      patch: 0,
      changeLog: false,
      numOfCodeComponents: 20,
      numOfCodeComponents_2: 20,
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
      window.addEventListener("click", (e) => {
        if (!menu.contains(e.target)) {
          this.menu = false;
        }
      });
    },
    showMusicMenu() {
      this.onPlayMusic = true;
      setTimeout(() => {
        let musicComponent = document.querySelector("#music-component");
        musicComponent.style.transition = "1s ease-out";
        musicComponent.style.bottom = "0px";
      }, 100);
    },
    closeMusicMenu() {
      let musicComponent = document.querySelector("#music-component");
      musicComponent.style.transition = "1s ease-out";
      musicComponent.style.bottom = "-100px";
      setTimeout(() => {
        this.onPlayMusic = false;
      }, 1000);
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
  border-inline: 1px solid $green;
  border-top: 1px solid $green;
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

#menu {
  position: absolute;
  right: 0;
  bottom: 0;
  width: 100px;
  height: 100px;
  border: 1px solid $green;
  box-shadow: inset 0px 0px 5px black;
  cursor: pointer;
  background: transparent;
  z-index: 1;
  transition: all 0.2s ease-in;
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
  font-size: 30px;
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
