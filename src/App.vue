<template>
  <div id="app">
    <div class="changeLog" v-if="changeLog">
      <button @click="changeLog = false">
        <span class="normal-text">Close</span>
      </button>
      <h1 class="text-center normal-text">Change Log</h1>
    </div>
    <div id="version" @mouseover="showChangeLog">{{ version }}</div>
    <div class="container-fluid">
      <div
        :class="'col col-' + i"
        v-for="(col, i) in numColumns"
        :key="i"
      ></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      major: 0,
      minor: 6,
      patch: 0,
      changeLog: false,
      codeLength: 5000,
      numColumns: 50,
      speedTypying: 50,
      matrixCodeText: "",
      matrixCodeArray: [],
      consoleMessage: {
        cleaned: {
          text: "%cCLEANED",
          styles: "background:black; color: #3cff3c; font-size:18px",
        },
        error: {
          text: "%cERROR",
          styles: "background:black; color: #ff3c3c; font-size:18px",
        },
      },
    };
  },
  mounted() {
    this.launchCodeGenerator();
  },
  created() {
    this.getMatrixCode();
  },
  computed: {
    version() {
      return `V${this.major}.${this.minor}.${this.patch}`;
    },
  },
  methods: {
    showChangeLog () {
      this.changeLog = true;
    },
    randomNumber(length) {
      return Math.floor(Math.random() * length);
    },
    randomNumberRange(from, to) {
      return Math.floor(Math.random() * (to - from + 1) + from);
    },
    showConsoleLog(type) {
      let { [type]: obj } = this.consoleMessage;
      console.log(obj.text, obj.styles);
    },
    randomizeMatrixCode() {
      // First, we will generate a string with random characters and we will store it in an array
      let characters = `qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM1234567890!#$%^&*()_+-=[]{}|;':",./<>?`;
      let arr = [];
      let string = "";
      for (let i = 0; i < this.codeLength; i++) {
        let random = Math.floor(Math.random() * characters.length);
        arr.push(characters[random]);
      }
      string = String(arr).replaceAll(",", "");
      this.matrixCodeText = string;
      return string;
    },
    getMatrixCode() {
      // Second, we will store in a 50 length array the 50 random strings
      for (let i = 0; i < this.numColumns; i++) {
        this.matrixCodeArray[i] = [];
        this.matrixCodeArray[i].push(this.randomizeMatrixCode());
      }
    },
    launchCodeGenerator() {
      // Third, we will generate the matrix code calling the typeCode function
      const SPEED = this.speedTypying;
      this.typeCode(SPEED);
    },
    randomizeStyles(element) {
      element.style.fontSize = `${this.randomNumberRange(14, 35)}px`;
      element.style.left = `${this.randomNumberRange(-200, 2120)}px`;
      element.style.top = `${this.randomNumberRange(-200, 100)}px`;
      element.style.opacity = `${this.randomNumberRange(0, 1).toString()}`;
    },
    // typeCode(SPEED) {
    //   let col_0 = document.querySelector(`.col-0`);
    //   let col_1 = document.querySelector(`.col-1`);
    //   let col_2 = document.querySelector(`.col-2`);
    //   let col_3 = document.querySelector(`.col-3`);
    //   let col_4 = document.querySelector(`.col-4`);
    //   let col_5 = document.querySelector(`.col-5`);
    //   const COLS = document.querySelectorAll(`.col`);
    //   let i = 0;
    //   let text = String(...this.matrixCodeArray);
    //   // Randomize styles of each column
    //   for (let col of COLS) {
    //     this.randomizeStyles(col);
    //   }
    //   const type = () => {
    //     if (i < this.codeLength) {
    //       setTimeout(() => {
    //         col_0.lastElementChild.style.color = "cyan";
    //       }, 100);
    //       col_0.innerHTML += `<p class="matrixCodeChar redChar">${text.charAt(
    //         this.randomNumber(text.length)
    //       )}</p>`;
    //       col_1.innerHTML += `<p class="matrixCodeChar">${text.charAt(
    //         this.randomNumber(text.length)
    //       )}</p>`;
    //       col_2.innerHTML += `<p class="matrixCodeChar">${text.charAt(
    //         this.randomNumber(text.length)
    //       )}</p>`;
    //       col_3.innerHTML += `<p class="matrixCodeChar">${text.charAt(
    //         this.randomNumber(text.length)
    //       )}</p>`;
    //       col_4.innerHTML += `<p class="matrixCodeChar">${text.charAt(
    //         this.randomNumber(text.length)
    //       )}</p>`;
    //       col_5.innerHTML += `<p class="matrixCodeChar">${text.charAt(
    //         this.randomNumber(text.length)
    //       )}</p>`;
    //       i++;
    //       setTimeout(type, this.randomNumberRange(SPEED, SPEED * 2));
    //     }
    //   };
    //   type(col_0);
    // },
    typeCode() {
      const COLS = document.querySelectorAll(`.col`);
      let i = 0;
      let j = 0;
      const type = (char_index, col_index) => {
        let text = String(...this.matrixCodeArray);
        const INDEX_COLUMN = document.querySelector(`.col-${col_index}`);
        if (char_index < this.codeLength) {
          let paragraph = document.createElement("p");
          paragraph.className = "matrixCodeChar";
          paragraph.innerHTML = `${text.charAt(
            this.randomNumber(text.length)
          )}`;
          INDEX_COLUMN.appendChild(paragraph);
          // setTimeout(type, this.randomNumberRange(SPEED, SPEED * 2));
          i++;
        }
      };
      COLS.forEach((col) => {
        this.randomizeStyles(col);
      });
      const typeEachColumn = () => {
        if (i < this.codeLength) {
          type(i, j);
          setTimeout(typeEachColumn, 100);
          i++;
          console.log("Char: " + i);
          // eslint-disable-next-line prettier/prettier
          if (i > this.randomNumberRange(0, 50)) {
            j++;
            i = 0;
          }
        }
        console.log(`Char: ${i}`);
        console.log(`Column: ${j}`);
      };
      typeEachColumn();
    },
  },
};
</script>

<style lang="scss">
// SCSS variables

$green: #3cff3c;
$cyan: #00ffff;

@font-face {
  font-family: "Matrix";
  src: url("./assets/fonts/matrix.ttf") format("truetype");
}

.col {
  height: 100%;
  width: 25px;
  position: absolute;
}

#app {
  position: absolute;
  overflow: hidden;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background-color: #151020;
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

.changeLog {
  text-align: center;
  width: 100%;
  z-index: 1;
  height: 100%;
  position: absolute;
  background: #e4dede;
  animation: fadeIn 0.3s ease-in-out;
}

.normal-text {
  font-family: "Consolas";
}

.redChar {
  color: red !important;
}

.matrixCodeChar {
  margin: 10px;
  padding: 0;
  opacity: 1;
  color: $green;
  text-align: center;
  font-weight: bold;
}

// p {
//   animation: fadeOut 1s;
// }

@keyframes fadeOut {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
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
