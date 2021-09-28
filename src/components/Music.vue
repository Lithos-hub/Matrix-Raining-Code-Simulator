<template>
<div>
  <div id="interface">
    <button id="close-button" @click="closeMenu">X</button>
    <button id="play-button" @click="toggleMusic">
      <img v-if="!onPlay" src="../assets/ico/play.svg" id="play-icon" />
      <img v-if="onPlay" src="../assets/ico/pause.svg" id="pause-icon" />
    </button>
    <audio id="audio" src="../assets/mp3/soundtrack.mp3" loop preload />
    <p class="music-timer">{{ trackTime }}</p>
    <div id="progress-bar-cover"></div>
  </div>
    <div id="progress-bar"></div>
</div>
</template>

<script>
export default {
  data() {
    return {
      onPlay: false,
      isPaused: Boolean,
      progressed: 0,
      i: 0,
      musicDuration: 0,
      audioLength: 0,
    };
  },
  computed: {
    // eslint-disable-next-line vue/return-in-computed-property
    trackTime() {
      let isPaused = this.isPaused;
      let formattedTime;
      let i = 0;
      if (!isPaused && this.audioLength > i) {
        // eslint-disable-next-line vue/no-async-in-computed-properties
        setTimeout(() => {
          this.audioLength--;
        }, 1000);
        formattedTime = this.formatTime(this.audioLength);
      }
      if (isPaused) {
        formattedTime = this.formatTime(this.progressed);
      }
      return formattedTime;
    },
  },
  methods: {
    closeMenu() {
      this.$emit("close-music-menu");
    },
    formatTime(time) {
      let minutes = Math.floor(time / 60);
      let seconds = Math.floor(time % 60);
      if (minutes < 10) {
        minutes = "0" + minutes;
      }
      if (seconds < 10) {
        seconds = "0" + seconds;
      }
      return minutes + ":" + seconds;
    },
    toggleMusic() {
      this.onPlay = !this.onPlay;
      let audio = document.querySelector("audio");
      let musicDuration = audio.duration;
      let maxWidth = 400;
      let progress = maxWidth / musicDuration;
      let progressBar = document.querySelector("#progress-bar");
      let id;
      setTimeout(() => {
        if (this.onPlay) {
          audio.play();
          this.isPaused = audio.paused;
          id = setInterval(onPlayingMusic, 100);
          this.audioLength = audio.duration;
          const onPlayingMusic = () => {
            if (this.onPlay && this.i <= musicDuration) {
              progressBar.style.width = `${10 + this.i * progress}px`;
              progressBar.style.maxWidth = "400px";
              progressBar.style.left = "100px";
              if (this.i % 5 === 0) {
                progressBar.style.marginLeft = `${this.i / 2}px`;
              }
              this.i++;
              setTimeout(onPlayingMusic, 1000);
            }
          };
          onPlayingMusic();
        } else {
          audio.pause();
          this.isPaused = audio.paused;
          this.progressed = this.audioLength - this.i;
          progressBar.style.width = `${this.i * progress}px`;
        }
        if (this.i === musicDuration) {
          this.i = 0;
          clearInterval(id);
        }
      }, 100);
    },
  },
};
</script>

<style lang="scss">
@import "../assets/scss/variables.scss";
@import "../assets/scss/app.scss";

#interface {
  z-index: 1;
}

.music-timer {
  z-index: 2;
  position: absolute;
  left: 10%;
  bottom: 0;
  font-size: 14px;
  color: $green;
  font-family: "Consolas";
}

#play-button {
  z-index: 2;
  cursor: pointer;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  position: absolute;
  background: transparent;
  left: 5%;
  top: 50%;
  transform: translate(-50%, -50%);
  padding: auto;
  margin: auto;
}

#pause-icon {
  z-index: 2;
  cursor: pointer;
  width: 20px;
  height: 20px;
  position: absolute;
  background: transparent;
  left: 0%;
  top: 40%;
  transform: translate(-50%, -50%);
  padding: 0;
  margin: 0;
}

#pause-icon {
  z-index: 2;
  position: relative;
  left: 40%;
  top: 35%;
  transform: translate(-50%, -50%);
}

#play-icon {
  z-index: 2;
  position: relative;
  left: 50%;
  top: 40%;
  transform: translate(-50%, -50%);
}

#progress-bar {
  position: absolute;
  opacity: 1;
  float: left;
  top: 50%;
  transform: translate(-50%, -50%);
  background: white;
  height: 5px;
  border-radius: 10px;
  box-shadow: 0px 0px 2px white, 0px 0px 10px $green;
  z-index: 0;
}

#progress-bar-cover {
  position: absolute;
  left: 0;
  z-index: 1;
  width: 100px;
  height: 100%;
  background: linear-gradient(120deg, #303030 5%, #151515);
}

#close-button {
  cursor: pointer;
  position: absolute;
  right: 0;
  border: 2px solid #151515;
  box-shadow: 0px 0px 10px black;
  padding-top: 15px;
  padding-bottom: 50px;
  height: 25px;
  border-top-right-radius: 10px;
  border-bottom-left-radius: 10px;
  background: rgb(111, 28, 28);
  color: white;
  transition: 0.3s;

  &:hover {
    background: rgb(179, 47, 47);
  }
}
</style>
