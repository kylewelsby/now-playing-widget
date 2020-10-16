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
        let apiKey = import.meta.env.VITE_LAST_FM_API_KEY;
        let res = await fetch(
          `//ws.audioscrobbler.com/2.0/?method=user.getrecenttracks&user=${this.username}&api_key=${apiKey}&format=json`
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
            this.artwork = artwork["#text"];
          }
        }
      }
    }
  };
</script>
