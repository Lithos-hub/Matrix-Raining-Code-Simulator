<template>
  <div class="changeLog" v-if="changeLog">
    <h1>Changelog</h1>
    <div
      class="button-wrapper"
      @mouseover="changeFontFamily"
      @mouseleave="changeFontFamily"
    >
      <button @click="changeLog = false" class="button">
        <span class="button-text">Close</span>
      </button>
    </div>
    <div class="changelog-list"></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      changeLog: true,
      changeLogList: [
        " ____________________ Version 0.5.0: ____________________  ",
        " Working raining code animation (with bugs and some crashes).",
        " ____________________ Version 0.6.0: ____________________  ",
        "  Added individual class to each character.",
        " ____________________ Version 0.7.0: ____________________  ",
        " Changed the algorithm to type the characters. ",
        " Added changeLog component.",
        " Improved the raining code animation.",
        " ____________________ Version 0.8.0: ____________________  ",
        " The raining code is now an iterative component.",
        " Infinite behavior has been added to the raining animation.",
        " ____________________ Version 0.8.5: ____________________  ",
        " Improved the performance.",
        " Added minor changes.",
      ],
    };
  },
  mounted() {
    this.transformText();
  },
  methods: {
    changeFontFamily() {
      let button = document.querySelector(".button-text");
      if (button.style.fontFamily === "Matrix") {
        button.style.fontFamily = "Consolas";
      } else {
        button.style.fontFamily = "Matrix";
      }
    },
    transformText() {
      let changes = [];
      let totalLength = 0;
      for (let string of this.changeLogList) {
        changes.push(string);
        totalLength += string.length;
      }
      let arrIndex = 0;
      let charIndex = 0;
      let p = document.createElement("p");
      let space = document.createElement("h2");
      const CHANGELOG_LIST = document.querySelector(".changelog-list");
      const type = (arrIndex, charIndex) => {
        if (arrIndex <= changes.length) {
          if (arrIndex <= totalLength) {
            p.classList.add("list-item");
            p.innerHTML += changes[arrIndex].charAt(charIndex);
            setTimeout(type, 5);
          }
        }
      };
      const typeEachItem = () => {
        if (charIndex <= totalLength) {
          setTimeout(typeEachItem, 10);
          charIndex++;
          type(arrIndex, charIndex);
          CHANGELOG_LIST.appendChild(p);
        }
        if (charIndex === changes[arrIndex].length) {
          charIndex = 0;
          p.appendChild(space);
          arrIndex++;
        }
      };
      typeEachItem();
    },
  },
};
</script>

<style lang="scss">
$green: #3cff3c;
$cyan: #00ffff;

.changeLog {
  text-align: center;
  transition: all 1s ease-in;
  width: 100%;
  z-index: 1;
  height: 100%;
  position: absolute;
  background: #151313;
  animation: fadeIn 0.3s ease-in-out;

  h1 {
    color: white;
    font-family: "Consolas";
    font-weight: lighter;
    letter-spacing: 10px;
  }
}

.button-wrapper {
  width: 200px;
  height: 40px;
  cursor: pointer;
  padding: auto;
  position: fixed;
  top: 90%;
  left: 50%;
  transform: translate(-50%, -50%);
  transition: 100ms ease-in;
  box-shadow: 0px 0px 10px rgb(42, 94, 29);

  &:hover {
    background: linear-gradient(120deg, #303030, rgb(46, 42, 42));
    color: white;
    font-family: "Consolas";
  }
}

.button {
  cursor: pointer;
  width: 200px;
  height: 40px;
  background: #151515;
  color: $green;
  letter-spacing: 10px;
  border: none;
  border-radius: 5px;
  font-weight: bold;
}

.version-title {
  color: $green;
  font-family: "Consolas";
  font-size: 30px !important;
}
.list-item {
  font-family: "Consolas";
  margin: 0 auto;
  color: $green;
  font-size: 20px;
  margin-block: 10px;
}
</style>
