<template>
  <div id="cratedigger">
    <div id="cratedigger-canvas"></div>
    <div id="cratedigger-loading">
      <div class="info-container">
        <div class="vertical-center">
          <span class="loading-label">Loading records...</span>
        </div>
      </div>
    </div>
    <div id="cratedigger-info">
      <div class="info-container">
        <div class="vertical-center">
          <div id="cratedigger-record-artist"></div>
          <div id="cratedigger-record-title"></div>
          <div id="cratedigger-record-year"></div>
          <div id="cratedigger-record-cover"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import cratedigger from "../assets/scripts/cratedigger.js";

export default {
  name: "Canvas",
  components: {},
  props: ["records"],
  data() {
    return {
      camera: null,
      scene: null,
      renderer: null,
      mesh: null,
    };
  },
  mounted() {
    this.init();
  },
  created() {},
  computed: {},
  methods: {
    init() {
      var self = this;
      console.log(this.records);
      cratedigger.init({
        debug: false,
        records: this.records,
        elements: {
          rootContainer: document.getElementById("cratedigger"),
          canvasContainer: document.getElementById("cratedigger-canvas"),
          loadingContainer: document.getElementById("cratedigger-loading"),
          infoContainer: document.getElementById("cratedigger-info"),
        },
        onInfoPanelOpened() {
          self.fillInfoPanel(cratedigger.getSelectedRecord());
        },

        onInfoPanelClosed() {},
      });

      cratedigger.loadRecords(this.records, true, () => {
        
      });
    },
    fillInfoPanel(record) {
      const titleContainer = document.getElementById(
        "cratedigger-record-title"
      );
      const artistContainer = document.getElementById(
        "cratedigger-record-artist"
      );
       const yearContainer = document.getElementById(
        "cratedigger-record-year"
      );
      const coverContainer = document.getElementById(
        "cratedigger-record-cover"
      );
      if (record.data.title) {
        titleContainer.innerHTML = record.data.title;
      }
      if (record.data.artist) {
        artistContainer.innerHTML = record.data.artist;
      }
      if (record.data.cover) {
        coverContainer.style.backgroundImage = "url(" + record.data.cover + ")";
      }
      if (record.data.year) {
        yearContainer.innerHTML = record.data.year;
      }
    },
  },
  watch: {},
};
</script>

<style scoped>
#cratedigger {
  width: 100%;
  height: 75vh;
  position: relative;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  overflow: hidden;
}

#cratedigger-canvas canvas.grab {
  cursor: -webkit-grabbing;
  cursor: -moz-grabbing;
}

#cratedigger-loading {
  background-color: #111111;
  z-index: 10;
  height: 100%;
  width: 100%;
  transition: opacity 1s;
}

#cratedigger-info {
  background-color: rgba(17, 17, 17, 0.9);
  z-index: 5;
  display: block;
  transition: opacity 0.6s;
}

#cratedigger-record-title {
  color: #fefefe;
  font-size: 40px;
  line-height: 45px;
  font-weight: 700;
  text-transform: uppercase;
}

#cratedigger-record-artist {
  color: #fefefe;
  font-size: 26px;
  font-weight: 700;
}

#cratedigger-record-year {
  color: #fefefe;
  font-size: 26px;
  font-weight: 700;
}

#cratedigger-record-cover {
  display: inline-block;
  width: 400px;
  height: 400px;
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center center;
  margin: 25px 0 50px;
}

.info-container {
  display: table;
  width: 100%;
  height: 100%;
}

.vertical-center {
  display: table-cell;
  vertical-align: middle;
  text-align: center;
}

.loading-label {
  color: #fefefe;
  font-size: 50px;
  line-height: 50px;
  font-weight: 700;
  text-transform: uppercase;
}

#bottom-bar {
  position: absolute;
  z-index: 8;
  background-color: rgba(17, 17, 17, 0.5);
  right: 0;
  bottom: 0;
  left: 0;
  height: 60px;
  width: 100%;
  display: table;
  color: #fff;
  overflow: hidden;
  transition: all 0.3s ease-in-out;
  table-layout: fixed;
}

#bottom-bar.closed {
  bottom: -60px;
}

#bottom-bar .bottom-bar-item {
  display: table-cell;
  text-align: center;
  text-transform: uppercase;
  line-height: 60px;
  font-size: 30px;
  transition: all 0.15s ease-in-out;
  overflow: hidden;
  color: #fff;
  cursor: pointer;
}

#bottom-bar .bottom-bar-item:hover {
  background-color: #fff;
  color: #111;
}

.noselect {
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
</style>