<template>
  <div id="app">
    <div id="top-bar">
      <input v-model="username" />
      <button @click="getReleases()">Fetch</button>
    </div>
    <Canvas v-if="!loading" :records="data" />
    <div v-if="loading">
      <h1>Loading...</h1>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Canvas from "./components/Canvas.vue";
require("dotenv").config();

axios.defaults.headers.post["Content-Type"] =
  "application/x-www-form-urlencoded";

export default {
  name: "App",
  components: { Canvas },
  methods: {
    getReleases() {
      this.data = [];
      this.loading = true;
      axios
        .get(
          `https://api.discogs.com/users/${this.username}/collection/folders/0/releases?key=${process.env.VUE_APP_KEY}&secret=${process.env.VUE_APP_SECRET}&per_page=500`
        )
        .then((res) => {
          // console.log(res);
          this.data = this.mapData(
            res.data.releases.sort((a, b) =>
              a.basic_information.artists[0].name >
              b.basic_information.artists[0].name
                ? 1
                : -1
            )
          );
        });
    },
    mapData(d) {
      let newData = [];
      d.forEach((val) => {
        newData.push({
          title: val.basic_information.title,
          artist: val.basic_information.artists[0].name,
          cover: val.basic_information.cover_image,
          year: val.basic_information.year,
          id: val.id,
          hasSleeve: false,
        });
      });
      this.loading = false;
      return newData;
    },
  },
  data() {
    return {
      data: [],
      loading: true,
      username: "beankonducta",
    };
  },
  mounted() {
    this.getReleases();
  },
};
</script>

<style>
html,
body,
div,
span,
applet,
object,
iframe,
h1,
h2,
h3,
h4,
h5,
h6,
p,
blockquote,
pre,
a,
abbr,
acronym,
address,
big,
cite,
code,
del,
dfn,
em,
img,
ins,
kbd,
q,
s,
samp,
small,
strike,
strong,
sub,
sup,
tt,
var,
b,
u,
i,
center,
dl,
dt,
dd,
ol,
ul,
li,
fieldset,
form,
label,
legend,
table,
caption,
tbody,
tfoot,
thead,
tr,
th,
td,
article,
aside,
canvas,
details,
embed,
figure,
figcaption,
footer,
header,
hgroup,
menu,
nav,
output,
ruby,
section,
summary,
time,
mark,
audio,
video {
  margin: 0;
  padding: 0;
  border: 0;
  font-size: 100%;
  font: inherit;
  vertical-align: baseline;
}
/* HTML5 display-role reset for older browsers */

article,
aside,
details,
figcaption,
figure,
footer,
header,
hgroup,
menu,
nav,
section {
  display: block;
}
body {
  line-height: 1;
}
ol,
ul {
  list-style: none;
}
blockquote,
q {
  quotes: none;
}
blockquote:before,
blockquote:after,
q:before,
q:after {
  content: "";
  content: none;
}
table {
  border-collapse: collapse;
  border-spacing: 0;
}

#app {
  width: 100%;
  height: 800px;
}

#top-bar {
  width: 100%;
  height: 10%;
  text-align: center;
}

.record {
  width: 25%;
  height: 10%;
  display: flex;
}

img {
  width: 50%;
  height: 100%;
  background: black;
  margin: 1%;
}
</style>