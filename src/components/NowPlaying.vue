<template>
  <div v-if="isActive">
    <NowPlayingTicker v-if="theme === 'ticker'" :artist="artist" :title="title" :artwork="artwork"/>
    <NowPlayingStacked v-else :artist="artist" :title="title" :artwork="artwork"/>
  </div>
  <div v-if="!username" class="text-red-500">Missing username from url example: <code>now-playing.app?user=halfcube</code></div>
</template>
<script>
  import NowPlayingStacked from './NowPlayingStacked.vue'
  import NowPlayingTicker from './NowPlayingTicker.vue'
  export default {
    components: {
      NowPlayingStacked,
      NowPlayingTicker
    },
    data() {
      return {
        counter: 0,
        theme: "stacked",
        isActive: true,
        username: null,
        title: "",
        artist: "",
        artwork: ""
      };
    },
    beforeMount() {
      window.location.search
      this.getParams()
      this.startPolling()
    },
    methods: {
      getParams() {
        let params = new URLSearchParams(window.location.search)
        this.username = params.get('user')
        this.theme = params.get('theme')
        this.apiKey = params.get('apiKey')
      },
      startPolling() {
        this.fetch();
        window.setTimeout(() => {
          window.requestAnimationFrame(() => {
              this.startPolling();
          });
        }, 2500);
      },
      async fetch() {
        if(!this.username) {
          return
        }
        let res = await fetch(
          `//ws.audioscrobbler.com/2.0/?method=user.getrecenttracks&user=${this.username}&api_key=${this.apiKey}&format=json&limit=1`
        );
        let data = await res.json();
        if (data.error) {
          this.isActive = false;
        } else {
          this.isActive = true;
          let track = data.recenttracks.track[0];
          this.artist = track.artist["#text"];
          this.title = track.name;
          let artwork = track.image.find((i) => i.size === "medium");
          if (artwork) {
            this.artwork = artwork["#text"] || 'https://lastfm.freetls.fastly.net/i/u/64s/4128a6eb29f94943c9d206c08e625904.jpg';
          } else {
            this.artwork = 'https://lastfm.freetls.fastly.net/i/u/64s/4128a6eb29f94943c9d206c08e625904.jpg';
          }
        }
      }
    }
  };
</script>
