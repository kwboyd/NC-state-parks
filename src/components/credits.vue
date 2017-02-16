<template>
  <div v-bind:class="modalObject" class="modal">
    <div class="modal-background"></div>
    <div class="modal-content"><div>
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
</style>
