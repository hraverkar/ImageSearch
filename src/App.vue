<template>
  <div id="app">
    <div class="container">
      <div class="button-wrapper">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <button class="button" style="vertical-align:middle" @click="searchUnsplash('Mountain')"><span>Mountain</span></button>
        <input v-model="message" placeholder="search me">
        <button class="button"  style="vertical-align:middle" @click="searchUnsplash(message)"><span>Search</span></button>
      </div>
              <label id="label"> {{Text}} </label>
      <stack
              :column-min-width="300"
              :gutter-width="15"
              :gutter-height="15"
              monitor-images-loaded
      >
        <stack-item
                v-for="(image, i) in images"
                :key="i"
                style="transition: transform 300ms"
        >
          <img :src="image.urls.small" :alt="image.alt_description" />
        </stack-item>
      </stack>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { Stack, StackItem } from "vue-stack-grid";

export default {
  name: "app",
  components: {
    Stack,
    StackItem
  },
  data: () => ({
    images: [],
    message:'',
    Text:''
  }),
  methods: {
    searchUnsplash(topic) {
      this.Text ='';
      this.images = [];
      axios
        .get(
          `https://api.unsplash.com/search/photos?query=${topic}&per_page=30`,
          {
            headers: {
              Authorization:
                "Client-ID 15ac77a08b3fac9e1431695b2b456f87f0b4dca0e1048a64c35e6e67109e4ed7",
              "Accept-Version": "v1"
            }
          }
        )
        .then(response => {
          this.images = response.data.results;
          if(this.images.length == 0){
            this.Text = 'No image found, Please search another keyword !! '
          }
        })
        .catch(() => {
          this.images = [];
        });
    }
  }
};
</script>
<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container {
  width: 80vw;
  margin: 0 auto;
}
img {
  width: 100%;
  height: auto;
  border-radius: 12px;
}
#label{
    vertical-align: middle;
    width:100%;
    text-align:center;
    height:150px;
    line-height:150px;
}
.button {
  display: inline-block;
  border-radius: 4px;
  background-color: #4CAF50;
  border: none;
  color: #FFFFFF;
  text-align: center;
  font-size: 28px;

  width: 150px;
  transition: all 0.5s;
  cursor: pointer;
  margin: 5px;
}

.button span {
  cursor: pointer;
  display: inline-block;
  position: relative;
  transition: 0.5s;
}

.button span:after {
  content: '\00bb';
  position: absolute;
  opacity: 0;
  top: 0;
  right: -20px;
  transition: 0.5s;
}

.button:hover span {
  padding-right: 25px;
}

.button:hover span:after {
  opacity: 1;
  right: 0;
}
</style>
