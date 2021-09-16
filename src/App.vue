<template>
  <div id="app">
    <div id="version">{{ version }}</div>
    <div class="container-fluid">
      <div class="code" v-for="(char, i) in greenCharList" :key="i"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      codeLength: 10,
      major: 0,
      minor: 6,
      patch: 0,
      counter: 0,
      speedTypying: 100,
      greenCharList: [],
      numCols: [],
    };
  },
  mounted() {
    this.typeCode();
  },
  created() {
    this.getMatrixCode();
  },
  computed: {
    version() {
      return `V${this.major}.${this.minor}.${this.patch}`;
    },
    height() {
      let clientWidth = window.screen.width;
      switch (true) {
        case clientWidth < 1824:
          return 450;
        case clientWidth > 1824:
          return 600;
        default:
          return 500;
      }
    },
  },
  methods: {
    randomize() {
      let greenChar;
      let greenChars = `qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM1234567890!#$%^&*()_+-=[]{}|;':",./<>?`;
      greenChar = greenChars[Math.floor(Math.random() * greenChars.length)];
      return greenChar;
    },
    randomNumber(length) {
      return Math.floor(Math.random() * length);
    },
    randomNumberRange(from, to) {
      return Math.floor(Math.random() * (to - from + 1) + from);
    },
    getMatrixCode() {
      for (let i = 0; i < this.codeLength; i++) {
        this.greenCharList[i] = [];
        this.greenCharList[i].push(this.randomize());
      }
    },
    typeCode() {
      const CODE_ELEMENTS = document.querySelectorAll(".code");
      // const CHILD_NODES = CODE_ELEMENTS[0].childNodes;
      const LENGTH = CODE_ELEMENTS.length;
      setInterval(() => {
        for (let i = 0; i < LENGTH; i++) {
          CODE_ELEMENTS[this.randomNumber(LENGTH)].style.position = "fixed";
          CODE_ELEMENTS[
            this.randomNumber(LENGTH)
          ].style.fontSize = `${this.randomNumberRange(12, 28)}px`;
          CODE_ELEMENTS[
            this.randomNumber(LENGTH)
          ].style.left = `${this.randomNumberRange(-100, 2000)}px`;
          CODE_ELEMENTS[
            this.randomNumber(LENGTH)
          ].style.top = `${this.randomNumberRange(0, -500)}px`;
          for (let j = 0; j < this.codeLength; j++) {
            let p = document.createElement("p");
            CODE_ELEMENTS[i].appendChild(p);
            setInterval(() => {
              p.classList.add("greenChar");
              p.classList.add(`char-col-${i}-no-${j}`);
              p.innerHTML = `
            ${this.greenCharList[this.randomNumber(this.codeLength)]}
        `;
              let eachPara = document.querySelector(`.char-col-${i}-no-${j}`);
              console.log(eachPara);
              // TODO: TO CONTINUE
            }, 1000);
          }
        }
        // }, 4000);
        // setInterval(() => {
        //   const LENGTH = PARA_ELEMENTS.length;
        //   for (let i = 0; i < LENGTH; i++) {
        //     PARA_ELEMENTS[i].style.transition = "opacity 0.5s";
        //     PARA_ELEMENTS[i].style.opacity = "0";
        //     setTimeout(() => {
        //       PARA_ELEMENTS[i].innerHTML = "";
        //     }, 3000);
        //   }
      }, 3000);
    },
  },
};
</script>

<style lang="scss">
// SCSS variables

$green: #3cff3c;
$cyan: #00ffff;

// use scss font
@font-face {
  font-family: "Matrix";
  src: url("./assets/fonts/matrix.ttf") format("truetype");
}

.code {
  position: relative;
  width: auto;
}

#app {
  position: absolute;
  overflow: hidden;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background-color: #100d18;
}

#version {
  font-family: "Consolas";
  font-size: 18px;
  color: $green;
  position: fixed;
  bottom: 0;
  left: 0;
  background: #151515;
  padding: 10px;
  opacity: 1;
}

* {
  font-family: "Matrix";
}

.firstChar {
  margin: 0;
  padding: 0;
  color: $cyan;
  text-align: center;
  font-weight: bold;
}

.greenChar {
  margin: 0;
  padding: 0;
  color: $green;
  text-align: center;
  font-weight: bold;
}

.firstChar {
  margin: 0;
  padding: 0;
  color: $cyan;
  text-align: center;
  font-weight: bold;
}

@keyframes fadeOut {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}
</style>
