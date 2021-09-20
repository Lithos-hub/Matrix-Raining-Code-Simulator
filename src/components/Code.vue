<template>
  <div class="container-fluid">
    <div
      :class="'col col-' + a + '-' + codeIndex"
      v-for="(col, a) in numColumns"
      :key="a + 'A'"
    ></div>
  </div>
</template>

<script>
export default {
  props: ["codeIndex"],
  data() {
    return {
      codeLength: 5000,
      numColumns: 10,
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
    this.typeCode(this.speedTypying);
  },
  created() {
    this.getMatrixCode();
  },
  methods: {
    randomNumber(length) {
      return Math.floor(Math.random() * length);
    },
    randomNumberRange(from, to) {
      return Math.floor(Math.random() * (to - from + 1) + from);
    },
    randomDecimalNumberRange(from, to) {
      return Math.random() * (to - from) + from;
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
    randomizeStyles(element) {
      let randomOpacity = parseFloat(
        this.randomDecimalNumberRange(0.5, 1).toFixed(1)
      );
      element.style.position = `fixed`;
      element.style.fontSize = `${this.randomNumberRange(20, 35)}px`;
      element.style.left = `${this.randomNumberRange(-100, 2000)}px`;
      element.style.top = `${this.randomNumberRange(-100, 100)}px`;
      element.style.opacity = randomOpacity;
    },
    typeCode(SPEED) {
      const COLS = document.querySelectorAll(`.col`);
      let paragraph;
      let i = 0; // ** => Code index character
      let j = 0; // ** => Column index
      // RANDOM STYLES FOR COLUMNS
      COLS.forEach((col) => {
        this.randomizeStyles(col);
      });
      // REMOVE COLUMNS
      const removeColumn = (col_index) => {
        const COLUMN = document.querySelector(
          `.col-${col_index}-${this.codeIndex}`
        );
        COLUMN.style.opacity = 0;
        COLUMN.innerHTML = "";
        setTimeout(removeColumn, 2000);
      };
      // DISPLAY COLUMN
      const displayColumn = (col_index) => {
        const COLUMN = document.querySelector(
          `.col-${col_index}-${this.codeIndex}`
        );
        COLUMN.style.opacity = 1;
      };
      // TYPE FUNCTION
      const type = (char_index, col_index) => {
        let text = String(...this.matrixCodeArray);
        const COLUMN = document.querySelector(
          `.col-${col_index}-${this.codeIndex}`
        );
        if (char_index < this.codeLength) {
          paragraph = document.createElement("p");
          paragraph.className = "matrixCodeChar";
          paragraph.innerHTML = `${text.charAt(
            this.randomNumber(text.length)
          )}`;
          COLUMN.appendChild(paragraph);
        }
      };
      // TYPE EACH COLUMN
      const typeEachColumn = () => {
        if (j <= this.numColumns) {
          if (i <= this.codeLength) {
            displayColumn(j);
            type(i, j);
            setTimeout(typeEachColumn, SPEED);
            i++;
            paragraph.style.opacity = "0";
          }
          if (i > this.randomNumberRange(50, 100)) {
            removeColumn(j);
            j++;
            i = 0;
          }
        }
        if (j === this.numColumns) {
          j = 0;
          i = 0;
        }
      };
      typeEachColumn();
    },
  },
};
</script>

<style lang="scss">
// SCSS variables

$green: #00ff00;
$cyan: #00ffff;

.col {
  height: 100%;
  width: 25px;
  position: fixed;
  top: 0;
}

.matrixCodeChar {
  margin: 10px;
  padding: 0;
  color: $green;
  text-align: center;
  font-weight: bold;
  animation: cyanToGreen 0.1s, fadeOut 2s ease;
}

@keyframes fadeOut {
  from {
    opacity: initial;
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

@keyframes cyanToGreen {
  from {
    color: $cyan;
  }
  to {
    color: $green;
  }
}
</style>
