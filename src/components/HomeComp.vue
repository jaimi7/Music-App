<template>
    <div class="container">
        <div class="header">
            <h2>
            Music App
            </h2>
        </div>
        <main>
            <div class="logo">
            <img :src="currentSong.imageSrc" alt="player">
            </div>
      <section class="player">
        <h1 class="title">{{ currentSong.title }} - <span>{{ currentSong.artist }}</span></h1>
        <div class="controls">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>The Playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == currentSong.src) ? 'song playing' : 'song'">
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main> 
    </div>
</template>

<script>

export default{
    name:'HomeComp',
    data () {
    return {
      currentSong: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: 'Run Free',
          artist: 'Charlie Puth',
          src: require('../assets/music/run-free.mp3'),
          imageSrc:require('../assets/img/player1.png')
        },
        {
          title: 'Sugar Crash',
          artist: 'BTS',
          src: require('../assets/music/sugar-crash.mp3'),
          imageSrc:require('../assets/img/player2.png')
        },
        {
          title: "We Don't Talk Anymore",
          artist: 'Selena Gomez',
          src: require("../assets/music/We-don't-talk-anymore.mp3"),
          imageSrc:require('../assets/img/player3.png')
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play (song) {
      if (typeof song.src != "undefined") {
        this.currentSong = song;
        this.player.src = this.currentSong.src;
      }
      this.player.play();
      this.player.addEventListener('ended', function () {
        this.index++;
        if (this.index > this.songs.length - 1) {
          this.index = 0;
        }
        this.currentSong = this.songs[this.index];
        this.play(this.currentSong);
      }.bind(this));
      this.isPlaying = true;
    },
    pause () {
      this.player.pause();
      this.isPlaying = false;
    },
    next () {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.currentSong = this.songs[this.index];
      this.play(this.currentSong);
    },
    prev () {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.currentSong = this.songs[this.index];
      this.play(this.currentSong);
    }
  },
  created () {
    this.currentSong = this.songs[this.index];
    this.player.src = this.currentSong.src;
  }
}
</script>

<style scoped>
.container{
    box-shadow: 0px 0px 10px 0px #ccc;
    width: 320px;
    min-height: 90vh;
    margin: 5vh auto;
}
.container .header{
    text-align: center;
    font-weight: bolder;
    color: #fff;
    padding: 15px;
    width: 100%;
    background: #000;
}
.logo{
    width: 120px;
    margin: 0px auto;
}
.logo img{
    width: 100%;
}
main {
  width: 100%;
  padding: 25px;
}
.title {
  color: #53565A;
  font-weight: 700;
  text-align: center;
}
.title span {
  font-weight: 400;
  font-style: italic;
}
.controls {
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  padding: 30px 0px;
}
button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}
button:hover {
  opacity: 0.8;
}
.play, .pause {
  font-size: 18px;
  font-weight: bold;
  padding: 10px 25px;
  margin: 0px 10px;
  border-radius: 5px;
  color: #fff;
  background-color: #7c0076;
}
.next, .prev {
  font-size: 14px;
  font-weight: bold;
  padding: 6px 20px;
  margin: 0px 10px;
  border-radius: 6px;
  color: #fff;
  background-color: #b100b1;
}
.playlist {
  padding: 30px 0px;
  border: 1px solid #ccc;
}
.playlist h3 {
  color: #212121;
  margin-bottom: 20px;
  text-align: center;
}
.playlist .song {
  display: block;
  width: 100%;
  padding: 10px;
  font-size: 15px;
  font-weight: 500;
  cursor: pointer;
  border-bottom: 1px solid #eeee;
}
.playlist .song:hover {
  color: #FF5858;
}
.playlist .song.playing {
  color: #fff;
  background-image: linear-gradient(to right, #CC2E5D, #FF5858);
}
</style>