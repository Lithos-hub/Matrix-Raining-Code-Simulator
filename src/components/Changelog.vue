<template>
  <div>
    <div class="changeLog">
      <h1>Changelog</h1>
      <div class="row">
        <div class="column">
          <h3 class="normal-text">GitHub repo</h3>
          <a href="https://github.com/Lithos-hub/matrix-raining-code">
            <img
              id="github-icon"
              src="@/assets/ico/github.svg"
              alt="GitHub"
              class="icon"
              width="50px"
              height="50px"
            />
          </a>
        </div>
        <div class="column">
          <div
            class="button-wrapper"
            @mouseover="changeFontFamily"
            @mouseleave="changeFontFamily"
          >
            <button @click="closeChangeLog" class="button">
              <span class="button-text">Close</span>
            </button>
          </div>
          <div class="changelog-list"></div>
        </div>
        <div class="column">
          <h3 class="normal-text">Made with</h3>
          <p class="normal-text">HTML</p>
          <p class="normal-text">CSS/SCSS</p>
          <p class="normal-text">JavaScript</p>
          <p class="normal-text">Vue.js</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["close-changelog"],
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
    closeChangeLog() {
      this.$emit("close-changelog");
    },
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
@import "../assets/scss/variables.scss";
@import "../assets/scss/app.scss";

.follow {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  animation: fadeInOut 5s ease;
}

.changeLog {
  text-align: center;
  transition: all 1s ease-in;
  width: 100%;
  z-index: 999;
  height: 100%;
  position: absolute;
  background: #151313;
  opacity: 0.8;
  animation: fadeInLowOpacity 2s ease-in-out;

  h1 {
    color: white;
    font-family: "Consolas";
    font-weight: lighter;
    letter-spacing: 10px;
  }
}

h2 {
  color: white;
  font-family: "Consolas";
  font-weight: lighter;
  letter-spacing: 20px;
}

#github-icon {
  padding: 15px;
  transition: all 0.3s ease-in;
  &:hover {
    box-shadow: inset 0px 0px 5px white, inset 0px 0px 20px #3cff3c;
    border: 3px solid white;
    border-radius: 50%;
  }
}
.button-text {
  font-family: "Matrix";
}

.button {
  font-family: "Matrix";
  cursor: pointer;
  width: 200px;
  height: 40px;
  position: fixed;
  top: 90%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: #151515;
  color: $green;
  letter-spacing: 10px;
  border: none;
  border-radius: 5px;
  font-weight: bold;

  &:hover {
    background: linear-gradient(120deg, #1a5b6f, rgb(18, 34, 26));
    .button-text {
      color: white;
    }
  }
}

.version-title {
  color: $green;
  font-family: "Consolas";
  font-size: 30px !important;
}
.list-item,
.normal-text {
  font-family: "Consolas";
  margin: 0 auto;
  color: $green;
  font-size: 20px;
  margin-block: 10px;
}

@keyframes fadeInLowOpacity {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 0.8;
  }
}
</style>
