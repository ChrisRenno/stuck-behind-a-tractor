<template>

  <header-component />
  <intro />
  <excuse v-if="excuse" :excuse="excuse" :message="message" :option="option" ref="excuse" />
  <more-excuses v-if="excuse" @moreLies="embellishExcuse($event)" />
  <input-component @optionChanged="updateOption($event)" @updatedExcuse="updateExcuse($event)" @updatedMessage="updateMessage($event)" @reset="reset()" ref="excuse" />
  
  <about-component v-show="showInfo" @showInfo="toggleInfo()"/>
  <footer-component @showInfo="toggleInfo()"/>
</template>

<script>
import HeaderComponent from './components/HeaderComponent.vue'
import Intro from './components/Intro.vue'
import InputComponent from './components/InputComponent.vue'
import Excuse from './components/Excuse.vue'
import MoreExcuses from './components/MoreExcuses.vue'
import AboutComponent from './components/AboutComponent.vue'
import FooterComponent from './components/FooterComponent.vue'

export default {
  name: 'App',
  components: {
    HeaderComponent,
    Intro,
    InputComponent,
    Excuse,
    MoreExcuses,
    AboutComponent,
    FooterComponent
  },
  emits: ['optionChanged','updatedExcuse', 'updatedMessage', 'moreLies', 'reset', 'showInfo'],
  data: () => ({
    option: 'offending obstruction',
    excuse: null,
    message: null,
    updatedExcuse: false,
    showInfo: false
  }),
  methods: {
    updateOption(option) {
      console.log('option updating in the parent')
      this.option = option
    },
    updateExcuse(excuse) {
      this.excuse = excuse
    },
    updateMessage(message) {   
      this.message = message
    },
    embellishExcuse(moreLies) {
      if (this.updatedExcuse === false) {
        this.excuse = this.excuse.slice(0, -1).concat(' but', moreLies, '.');
        this.updatedExcuse = true;
      } else {
        this.excuse = this.excuse.slice(0, -1).concat(' and ', moreLies, '.');
      }
    },
    reset() {
      this.option = null
      this.excuse = null
      this.message = null
      this.updatedExcuse = false
      window.scrollTo(0, 0);
    },
    toggleInfo() {
      this.showInfo = !this.showInfo
    }
  }
}
</script>

<style>
#app {
  display: flex;
  flex-direction: column;
  min-height: calc(100vh - 80px);
}
body {
  margin: 0;
  height: 100%;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  padding-bottom: 80px;
  position: relative;
  background: #F7F6F3;
}

section {
  padding: 16px 32px;
  max-width: 700px;
  /* width: 100%; */
  margin: 0 auto;
}
</style>
