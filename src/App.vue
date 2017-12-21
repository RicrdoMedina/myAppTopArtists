<template lang="pug">
  #app
    img(src='https://rcrdmedina.github.io/myAppTopArtists/dist/logo.png')
    h1 Project Music
    select(v-model="selectedCountry")
      option(v-for="country in counties" v-bind:value="country.value") {{ country.name }}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")
</template>

<script> 
import Artist from './components/Artist.vue'
import Spinner from './components/Spinner.vue'
import getArtists from './api'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      counties: [
        { name : 'Argentina', value : 'argentina' },
        { name : 'Colombia', value : 'colombia' },
        { name : 'España', value : 'spain' }
      ],
      selectedCountry: 'argentina',
      loading: true
    }
  },
  components:{
    Artist,
    Spinner
  },
  methods: {
    refreshArtist() {
      const self = this
      this.loading = true
      this.artists = []
      getArtists(this.selectedCountry)
        .then( function (artists) {
          self.artists = artists
          self.loading = false
        })
    }
  },
  mounted () {
   this.refreshArtist()
  },
  watch: {
    selectedCountry () {
      this.refreshArtist()
    }
  }
}
</script>

<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px

h1, h2
  font-weight normal

ul
  list-style-type none
  padding 0

li
  display inline-block
  margin 0 10px

a
  color #42b983
</style>
