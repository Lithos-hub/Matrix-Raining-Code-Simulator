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
      codeLength: 5000,
      major: 0,
      minor: 5,
      patch: 0,
      counter: 0,
      speedTypying: 50,
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
    getMatrixCode() {
      for (let i = 0; i < 100; i++) {
        this.greenCharList[i] = [];
        this.greenCharList[i].push(this.randomize());
      }
    },
    typeCode() {
      const CODE_ELEMENTS = document.querySelectorAll(".code");
      CODE_ELEMENTS.forEach((element) => {
        element.innerHTML = "";
        element.style.position = "absolute";
        element.style.left = `${this.randomNumber(1920)}px`;
        element.style.top = `${this.randomNumber(1080)}px`;
        let p = document.createElement("p");
        element.appendChild(p);
        setInterval(() => {
          p.classList.add(".greenChar");
          p.innerHTML += `
          <p class="greenChar" style="animation: fadeOut 3s">${
            this.greenCharList[this.randomNumber(100)]
          }</p>
        `;
        }, this.speedTypying);
      });

      const GREEN_CHAR_ELEMENTS = document.querySelectorAll("p");
      for (let i = 0; i < GREEN_CHAR_ELEMENTS.length; i++) {
        setInterval(() => {
          GREEN_CHAR_ELEMENTS[
            this.randomNumber(GREEN_CHAR_ELEMENTS.length)
          ].classList.add("fadeOut");
        }, 500);
      }
      setInterval(() => {
        const CODE_ELEMENTS = document.querySelectorAll(".code");
        CODE_ELEMENTS.forEach((element) => {
          element.innerHTML = "";
          element.style.position = "absolute";
          element.style.left = `${this.randomNumber(1920)}px`;
          element.style.top = `${this.randomNumber(1080)}px`;
          let p = document.createElement("p");
          element.appendChild(p);
          setInterval(() => {
            p.classList.add(".greenChar");
            p.innerHTML += `
            <p class="greenChar" style="animation: fadeOut 3s">${
              this.greenCharList[this.randomNumber(100)]
            }</p>
          `;
          }, this.speedTypying);
        });

        const GREEN_CHAR_ELEMENTS = document.querySelectorAll("p");
        for (let i = 0; i < GREEN_CHAR_ELEMENTS.length; i++) {
          setInterval(() => {
            GREEN_CHAR_ELEMENTS[
              this.randomNumber(GREEN_CHAR_ELEMENTS.length)
            ].classList.add("fadeOut");
          }, 500);
        }
        console.log("START");
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

.greenChar {
  transition: all 2s;
  display: block;
  width: auto;
  margin: 0 auto;
  color: $green;
  font-size: 25px;
  text-align: center;
  font-weight: bold;
}

.fadeOut {
  transition: all 1s ease-in-out;
  opacity: 0;
}

// ANIMATIONS

@keyframes rain {
  0% {
    top: -5%;
  }
  100% {
    top: 100%;
  }
}

// @keyframes cyanToGreen {
//   0% {
//     color: $cyan;
//   }
//   100% {
//     color: $green;
//   }
// }

@keyframes fadeOut {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}
</style>
