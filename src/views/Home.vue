<template>
  <div>
    <v-container class="home-width">
      <div class="py-5">
        <p>We investigate the spoken language of Tunisia’s youth.<br/>
We create a corpus of spoken language.<br/>
We compile a diachronic dictionary and<br/>
we work on tools and methods beneficial for research in the field of Arabic studies and beyond.<br/>
<p>Our project is grounded in a combination of dialectological approaches and up-to-date text technological methodologies.<br/>
Our project assigns particular importance to the dictionary/corpus interface.<br/>
Our project is conducted in the spirit of open source and open access. Therefore, both the corpus and the lexicographical data of the project will be made available to the scientific community through a publicly accessible web interface which will enable other scholars to do further analyses and to reuse the material.<br/>
Our project is based at two research institutions, the Institute for Near Eastern Studies (University of Vienna) and the Austrian Centre for Digital Humanities (Austrian Academy of Sciences).</p>
      </div>
    </v-container>
    <div class="text-center py-10 indigo lighten-5" v-if="mainData.hideCookieConsent">
      <h3 class="text-h5 font-weight-bold">Explore TUNICO</h3>
      <div class="d-flex align-center search-frm mt-4">
        <v-text-field @keyup.enter.native="search" v-model="mainData.search.value" label="Search TUNICO" class="mr-3" data-testid="quickSearch"></v-text-field>
        <v-btn @click="search" color="indigo darken-4 white--text">Search</v-btn>
      </div>
      or
      <div class="mt-4">
        <v-btn to="/tool" color="indigo darken-4 white--text">Browse</v-btn>
      </div>
    </div>
    <div class="text-center py-10 indigo lighten-5" v-else>
      <h3 class="text-h5 font-weight-bold">COOKIES</h3>
      <p>This project uses <b>website tracking and cookies</b> to collect statistical information about our users.<br/>
      This information is only used to enhance the user experience. The information is not shared with any third party.<br/>
      We need your consent to gather this information.</p>
      <p>You can find more information in the "Data privacy notice" of our "Site Notice".</p>
      <v-btn color="indigo darken-4 white--text" v-bind:x-large="true" v-on:click="cookiesAndTrackingAccepted" data-testid="acceptTracking">I accept</v-btn><br/><br/>
      <v-btn color="indigo darken-4 white--text" v-bind:small="true" v-on:click="cookiesAndTrackingRejected" data-testid="rejectTracking">I dont't want this</v-btn>
    </div>
    <div class="text-center py-5">
      <p>The web interface for TUNICO was developed in the <a href="https://voice.acdh.oeaw.ac.at/" target="_blank">VOICE CLARIAH project</a></p>
    </div>
  </div>
</template>

<script>
import { timer } from 'vue-timers'

export default {
  name: 'Home',
  props: {
    'mainData': Object
  },
  data: () => ({
    publicPath: process.env.BASE_URL
  }),
  mounted () {
    this.mainData.hideCookieConsent = this.$matomo && this.$matomo.hasRememberedConsent()
  },
  timers: {
    checkMatomo: { time: 100, autostart: true, repeat: true }
  },
  methods: {
    search () {
      this.$router.push('/tool')
      this.mainData.search.now = true
    },
    cookiesAndTrackingAccepted () {
      this.$matomo &&
      this.$matomo.rememberConsentGiven(720) &&
      this.$matomo.rememberCookieConsentGiven(720) &&
      this.$matomo.forgetUserOptOut()
      this.mainData.hideCookieConsent = true
    },
    cookiesAndTrackingRejected () {
      this.$matomo &&
      this.$matomo.optUserOut()
      this.mainData.hideCookieConsent = true
    },
    checkMatomo () {
      if (this.$matomo) {
        this.mainData.hideCookieConsent ||= this.$matomo.hasRememberedConsent()
        this.$timer.stop('checkMatomo')
      }
    },
  },
}
</script>

<style scoped>
  .search-frm {
    margin-left: auto;
    margin-right: auto;
    max-width: 400px;
  }
</style>
