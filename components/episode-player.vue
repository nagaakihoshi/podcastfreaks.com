<template lang="pug">
.episode
  a.play(@click="play()") {{ playerText }}
  a-blank.title(:href="episode.link") {{ episode.title }}
</template>

<style lang="sass" scoped>
.episode
  height: 60px
  display: flex
  flex-wrap: wrap
  flex-direction: column
  border-bottom: 1px solid #333
  .play
    width: 60px
    height: 60px
    background: #000
    display: flex
    justify-content: center
    align-items: center
    cursor: pointer
    color: white
    background-color: #7f00ff
    &:hover
      background-color: lighten(#7f00ff, 10)
    &:active
      background-color: lighten(#7f00ff, 20)
  .title
    height: 60px
    width: calc(100% - 100px)
    padding: 0 20px
    color: #aaa
    display: flex
    align-items: center
    overflow: hidden
    white-space: nowrap
    text-overflow: ellipsis
</style>

<script>
export default {
  props: {
    episode: {
      required: true,
      type: Object
    }
  },
  data: function() {
    return {
      playerText: '▶',
      player: null,
      timer: null
    }
  },
  mounted: function() {
    this.player = new Audio(this.episode.enclosure.$.url);
  },
  methods: {
    stop: function() {
      this.player.pause()
      this.player.currentTime = 0
      this.playerText = '▶'
      clearInterval(this.timer)
      this.timer = null
    },
    play: function() {
      if(this.player.paused) {
        this.player.play()
        this.$emit('play', this)
        if(this.timer == null) {
          this.playerText = 60
          let me = this
          this.timer = setInterval(function() {
            if(!me.player.paused) {
              me.playerText -= 1
              if(me.playerText <= 0) {
                me.stop()
              }
            }
          }, 1000)
        }
      }
      else {
        this.stop()
      }
    }
  }
}
</script>