<template>
  <footer class="footer-wrapper">
    <div class="footer-container">
      <p>Site by <a class="foot-link" href="http://kwboyd.com">Kate Boyd.</a></p>
      <p class="foot-middle">Visit my <a class="foot-link" href="http://github.com/kwboyd">Github.</a></p>
      <p>View info <a class="foot-link" @click="modalOpen">credits.</a></p>
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
.footer-wrapper {
  padding: .5rem;
}

.footer-container {
  display: flex;
  justify-content: center;
  align-items: center;
}

.foot-middle {
  margin: 0 8px;
}

.foot-link {
  color: #143a44;
  font-weight: bold;
}
</style>
