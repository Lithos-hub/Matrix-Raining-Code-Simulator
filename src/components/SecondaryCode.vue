<template>
  <div id="secondary-code" class="container-fluid">
    <div
      :class="'col-2 col-secondary-' + a + '-' + codeIndex"
      v-for="(col, a) in numColumns"
      :key="a + 'B'"
    ></div>
  </div>
</template>

<script>
export default {
  props: ["codeIndex", "custom-char-class"],
  data() {
    return {
      codeLength: 100,
      numColumns: 5,
      speedTypying: 100,
      matrixCodeText: "",
      matrixCodeArray: [],
    };
  },
  mounted() {
    this.typeCode(this.speedTypying);
  },
  created() {
    this.getMatrixCode();
  },
  computed: {
    width() {
      return window.screen.width;
    },
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
        this.randomDecimalNumberRange(0.2, 0.5).toFixed(1)
      );
      element.style.position = `fixed`;
      element.style.fontSize = `${this.randomNumberRange(8, 16)}px`;
      element.style.left = `${this.randomNumberRange(
        -100,
        this.width + 100
      )}px`;
      element.style.top = `${this.randomNumberRange(-100, 0)}px`;
      element.style.opacity = randomOpacity;
    },
    typeCode(SPEED) {
      const COLS = document.querySelectorAll(`.col-2`);
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
          `.col-secondary-${col_index}-${this.codeIndex}`
        );
        if (COLUMN !== null) {
          COLUMN.style.opacity = 0;
          setTimeout(() => {
            COLUMN.innerHTML = "";
          }, 4000);
        }
        setTimeout(removeColumn, 1000);
      };
      // DISPLAY COLUMN
      const displayColumn = (col_index) => {
        const COLUMN = document.querySelector(
          `.col-secondary-${col_index}-${this.codeIndex}`
        );
        COLUMN.style.opacity = 1;
      };
      // TYPE FUNCTION
      const type = (char_index, col_index) => {
        let text = String(...this.matrixCodeArray);
        const COLUMN = document.querySelector(
          `.col-secondary-${col_index}-${this.codeIndex}`
        );
        if (char_index < this.codeLength) {
          paragraph = document.createElement("p");
          paragraph.className = this.customCharClass;
          paragraph.innerHTML = `${text.charAt(
            this.randomNumber(text.length)
          )}`;
          COLUMN.appendChild(paragraph);
        }
      };
      // TYPE EACH COLUMN
      const typeEachColumn = () => {
        if (j < this.numColumns) {
          if (i < this.codeLength) {
            displayColumn(j);
            type(i, j);
            i++;
            paragraph.style.opacity = "0";
            setTimeout(typeEachColumn, SPEED);
          }
          if (i >= this.randomNumberRange(80, this.codeLength)) {
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
      // setInterval(typeEachColumn, 3000);
    },
  },
};
</script>

<style lang="scss">
@import "../assets/scss/variables.scss";

#secondary-code {
  display: flex;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 0;
  width: 100%;
  height: 100%;
}
</style>
