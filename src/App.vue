<template lang="pug">
  #app
    img(src='./assets/logo.png')
    h1 Platzimusic
    select(v-model="selectedCountry")
      option(v-for="country in countries" :value="country.value") {{ country.name}}
    ul
      Artist(v-for="artist in artists" :artist="artist" :key="artist.mbid")
</template>

<script>
import getArtists from './api'
import Artist from './components/Artist'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        {name: 'Argentina', value: 'argentina'},
        {name: 'Colombia', value: 'colombia'},
        {name: 'Spain', value: 'spain'},
      ],
      selectedCountry: 'argentina',
    }
  },
  components: {
    Artist,
  },
  methods: {
    refreshArtist() {
      const self = this
      this.artists = []
      getArtists(this.selectedCountry)
        .then(function (artists) {
          self.artists = artists
        });
    }
  },
  mounted() {
    this.refreshArtist()
  },
  watch: {
    selectedCountry() {
      this.refreshArtist()
    }
  },
}
</script>

<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #35495E
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