<template>
  <div id="app">
  
  <header>
    <h1>VueCloud</h1>
  </header>
  <main>
    <section class="player">
      <img src="./assets/DuaLipa.jpg" alt="">
      <h2 class="song-title">{{current.title}}  <span>{{current.artist}}</span></h2>
      
      <div class="controls">
        <button class="prev" @click="prev">Prev</button>
        <button class="play"  v-if="!isPlaying" @click="play">Play</button>
        <button class="pause" v-else @click="pause">Pause</button>
        <button class="next" @click="next">Next</button>
      </div>
    </section>

    <section class="playlist">
      <h3>Album</h3>
      <button v-for="song in songs" 
      :key="song.src" 
      @click="play(song)" 
      :class="(song.src
      == current.src)  ? 'song playing' : 'song'">
        {{ song.title }} - {{song.artist}}
      </button>
    </section>

  </main>

  <footer>
    <a href="https://vuejs.org/">www.vuejs.org</a>
  </footer>

  </div>

</template>
<script>
export default {
  name: 'App',
  data () {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
        title: 'Last Dance',
        artist: 'Dua Lipa',
        src: require('./assets/Dua Lipa - Last Dance.mp3')
        },
        {
          title: 'Losing You',
          artist: 'Boy Pablo',
          src: require('./assets/boy pablo - Losing You.mp3')
        },
      ],
    player: new Audio()
    }
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;

        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener('ended', function () {
        this.index++;

        if(this.index > this.songs.length -1) {
          this.index = 0;
      }

        this.current = this.songs[this.index];
        this.play(this.current);
      }.bind(this));
      this.isPlaying = true;
    },
    pause () {
      this.player.pause();
      this.isPlaying = false;
    },
    next () {
      this.index++;
      if(this.index > this.songs.length -1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev () {
      this.index--;
      if(this.index < 0) {
        this.index = this.songs.length - 1;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created () {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  }
}
</script>



<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;

}

body {
  font-family: helvetica;
}

header,footer {
  display: flex;
  justify-content: center;
  align-content: center;
  padding: 15px;
  background-color: #1AB954;
  color: #fff;
}


footer a {
  color: white;
  text-decoration: none;
  letter-spacing: 2.4px;
  font-size: 14.5px;

}

::selection {
  color:white;
  background-color:lightgreen;
}

img {
  display: flex;
  justify-content: center;
  align-content: center;
  margin-left: auto;
  margin-right: auto;
  padding: 2rem;
  box-shadow: 0px 0px 0px 0px rgba(0,0,0,0);
  transition: 0.7s;
}

img:hover {
  transition: 0.7s;
  box-shadow: 15px 20px 1px 1px rgb(213, 255, 220);
  margin-bottom: 2rem;
  padding: 2rem;
  cursor:pointer;
}

main {
  width: 100%;
  max-width: 768px;
  margin: 25px auto;
  padding: 25px;
  box-shadow: -5px 0px 75px 5px rgba(0,0,0,0.08);
  border-radius: 3rem;
}

.song-title {
  color: #131313;
  font-size: 2rem;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
  border-bottom: solid #838383 0.08px;
  padding: 20px;
}

.song-title span {
  font-weight: 100;
  font-size: 20px;
  display: flex;
  justify-content: center;
}

.controls {
  display:flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;
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
  transition: ease-in-out 0.5s;
}

.play, .pause {
  font-size: 20px;
  font-weight: 100;
  padding: 10px 90px;
  margin: 0px 15px;
  border-radius: 20px;
  color: #FFF;
  background-color: #131313;
}

.next, .prev {
  font-size:16px;
  font-weight: 100;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 20px;
  color: #FFF;
  background-color: #474747;
}



.playlist {
  padding: 0px 50px;
  transition: 0.5s;

}

.playlist h3 {
  color: #212121;
  font-size: 20px;
  font-weight: 900;
  text-transform: uppercase;
  margin-bottom: 30px;
  text-align: center;
}

.playlist .song {
  display: block;
  border-radius: 2rem;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}

.playlist .song:hover {
  color: #131313;
}

.playlist .song.playing {
  border-radius: 2rem;
  color: #FFF;
  background-image: linear-gradient(to right, #1AB954, #1BD760);
}


</style>
