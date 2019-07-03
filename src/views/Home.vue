<template lang="pug">
v-container#root(fluid fill-height)
  v-layout.justify-center.align-center
    v-card#card-main.mb-5
      v-card-title.subheading.justify-center.ma-3 
        span かわいそうなおめシスはかわいい
        v-btn(flat icon @click="favorite")
          v-icon(small :color="heartColor") favorite
          span.caption {{ favCount }}
      v-card-actions.justify-center
        v-btn.btn(outline xs6 color="pink" @click="pressRay") RAY
        v-btn.btn(outline xs6 color="blue" @click="pressRio") RIO
      v-card-text
        v-card#card-message
          v-card-text
            p {{ message }}
      v-card-actions.justify-center
        v-btn.btn(round color="blue lighten-1" @click="tweet")
          span.white--text tweet
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import axios from 'axios';

@Component({
  components: {
  },
})
export default class Home extends Vue {
  private API: string = 'https://script.google.com/macros/s/AKfycbx0qeWL2O9WQxHzj6t24FYUmJ87qg6S83dx4lRPCFpQ_XcpBak/exec';
  private favCount: number = 0;
  private heartColor: string = '';

  private message: string = '';

  mounted() { this.getFav(); }

  pressRay() { this.getMessage('レイ'); }

  pressRio() { this.getMessage('リオ'); }

  favorite() {
    this.setFav();
  }

  tweet() {
    const EUC = encodeURIComponent;
    const LINK = window.location.href;
    const message = this.message === '' ? 'かわいそうなおめシスはかわいい' : this.message;
    const hashtag = "omesis_kawaii"
    const URL = `https://twitter.com/intent/tweet?text=${EUC(message)}&hashtags=${EUC(hashtag)}&url=${LINK}`
    if (navigator.userAgent.indexOf('iPhone') > 0 || navigator.userAgent.indexOf('iPad') > 0 || navigator.userAgent.indexOf('iPod') > 0 || navigator.userAgent.indexOf('Android') > 0) {
        location.href = URL
    }else{
        window.open(URL, "_blank","top=50,left=50,width=500,height=500,scrollbars=1,location=0,menubar=0,toolbar=0,status=1,directories=0,resizable=1");
    }
  }

  private getFav() {
    axios.get(this.API)
      .then(response => {
        this.favCount = response.data.fav;
        this.heartColor = this.favCount&1 ? 'pink' : 'blue';
      })
  }

  private setFav() {
    axios.post(this.API)
      .then(response => {
        this.favCount = response.data.fav;
        this.heartColor = this.favCount&1 ? 'pink' : 'blue';
      })
  }
  
  private getMessage(name: string) {
    axios.get(this.API + '?name=' + name)
      .then(response => {
        this.message = response.data.message;
      })
  }
}
</script>

<style lang="stylus" scoped>
#root
  background: url('../assets/omesis.png') no-repeat
  background-size: contain
  background-position: center center

#card-main
  opacity: 0.9
  width: 80vw

#card-message
  max-width: 80vw

.btn
  width: 150px 
</style>
