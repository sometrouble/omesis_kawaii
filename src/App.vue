<template lang="pug">
  v-app
    v-toolbar(app dense)
      v-toolbar-title.title
        span #omesis_kawaii
      v-spacer
      v-btn(
        flat
        href="https://github.com/sometrouble/omesis_kawaii"
        target="_blank"
      ) GitHub
      v-btn(
        small
        depressed
        href="https://www.youtube.com/channel/UCNjTjd2-PMC8Oo_-dCEss7A"
        target="_blank"
        color="red"
      ) 
        v-icon(color="white") play_arrow
    v-content
      router-view
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';

@Component({
  components: {
  },
})
export default class App extends Vue {}
</script>
