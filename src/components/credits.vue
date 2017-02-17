<template>
  <div :parks="parks" v-bind:class="modalObject" class="modal">
    <div class="modal-background"></div>
    <div class="modal-content">
      <div id="modal-stuff">
        <p>This site uses Vue.js, as well as the Google Maps API.</p>
        <p>Information from <a href="ncparks.gov">NCParks.gov</a></p>
        <p>Images from:</p>
        <ul>
          <li v-for="park in parks">{{ park.source }}</li>
        </ul>
      </div>
    <div>
    <button @click="closeModal" class="modal-close"></button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      modalObject: {
        'is-active': false
      }
    }
  },
  props: ['parks'],
  methods: {
    showModal () {
      this.modalObject['is-active'] = true
    },
    closeModal () {
      this.modalObject['is-active'] = false
    }
  },
  mounted () {
    this.$evt.$on('openModal', this.showModal)
  },
  beforeDestroy () {
    this.$evt.$off('openModal', this.showModal)
  }
}
</script>

<style>
#modal-stuff {
  color: #fff;
}
</style>
