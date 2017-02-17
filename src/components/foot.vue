<template>
  <footer id="footer-wrapper">
    <div id="footer-container">
      <p>Site by <a class="foot-link" href="http://kwboyd.com">Kate Boyd.</a></p>
      <p id="foot-middle">Visit my <a class="foot-link" href="http://github.com/kwboyd">Github.</a></p>
      <button class="colored-button" @click="modalOpen">Credits</button>
    </div>
  </footer>
</template>

<script>
export default {
  methods: {
    modalOpen () {
      this.$evt.$emit('openModal')
    }
  }
}
</script>

<style>
#footer-wrapper {
  background-color: #fff;
  padding: .5rem;
}
#footer-container {
  display: flex;
  justify-content: center;
  align-items: center;
}
#foot-middle {
  margin: 0 8px;
}
.foot-link {
  color: #73c8ca;
}
</style>
