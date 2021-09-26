<template>
  <div id="interface">
    <button id="close-button" @click="closeMenu">X</button>
    <button id="play-button" @click="playMusic">
      <img v-if="!onPlay" src="../assets/ico/play.svg" id="play-icon" />
      <img v-if="onPlay" src="../assets/ico/pause.svg" id="pause-icon" />
    </button>
    <audio id="audio" src="../assets/mp3/soundtrack.mp3" />
    <div id="progress-bar"></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      onPlay: false,
      onPaused: true,
      progressed: 0,
    };
  },
  methods: {
    closeMenu() {
      this.$emit("close-music-menu");
    },
    playMusic() {
      this.onPlay = !this.onPlay;
      this.onPaused = !this.onPaused;
      let audio = document.querySelector("audio");
      let musicDuration = 350;
      let maxWidth = 400;
      let progress = maxWidth / musicDuration;
      let progressBar = document.querySelector("#progress-bar");
      let id;
      let i = 0;
      progressBar.style.opacity = "1";
      setTimeout(() => {
        audio.play();
        if (this.onPlay) {
          id = setInterval(onPlayingMusic, 1000);
          const onPlayingMusic = () => {
            if (i <= musicDuration && this.onPlay) {
              progressBar.style.width = `${i * progress}px`;
              i++;
              setTimeout(onPlayingMusic, 1000);
            } else {
              clearInterval(id);
            }
          };
          onPlayingMusic();
        } else {
          progressBar.style.width = `${this.progressed}px`;
        }
      }, 500);
    },
  },
};
</script>

<style lang="scss">
@import "../assets/scss/variables.scss";
@import "../assets/scss/app.scss";

#interface {
  z-index: 3;
}

#play-button {
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
  position: relative;
  left: 40%;
  top: 35%;
  transform: translate(-50%, -50%);
}

#play-icon {
  position: relative;
  left: 50%;
  top: 40%;
  transform: translate(-50%, -50%);
}

#progress-bar {
  opacity: 0;
  position: absolute;
  float: left;
  left: 52%;
  top: 50%;
  transform: translate(-50%, -50%);
  background: white;
  max-width: 400px;
  height: 5px;
  border-radius: 10px;
  box-shadow: 0px 0px 2px white, 0px 0px 10px $green;
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
