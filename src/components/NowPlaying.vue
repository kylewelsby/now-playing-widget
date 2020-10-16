<template>
  <div class="flex flex-row items-center text-shadow" v-if="isActive">
    <div class="mr-4">
      <img :src="artwork" class="w-16 rounded-lg shadow-lg" />
    </div>
    <!-- Artwork -->
    <div>
      <div class="font-semibold text-white">{{ title }}</div>
      <!-- title -->
      <div class="text-gray-100">{{ artist }}</div>
      <!-- artist -->
    </div>
    <!-- Artist & Titles -->
  </div>
</template>
<script>
  export default {
    data() {
      return {
        counter: 0,
        isActive: true,
        username: "halfcube",
        title: "",
        artist: "",
        artwork: ""
      };
    },
    beforeMount() {
      this.startPolling()
    },
    methods: {
      startPolling() {
        this.fetch();
        window.setTimeout(() => {
          window.requestAnimationFrame(() => {
              this.startPolling();
          });
        }, 2500);
      },
      async fetch() {
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
