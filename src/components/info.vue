<template>
  <div :parks="parks" class="container">
      <h1> {{ parks.name }} </h1>
      <h2> {{ parks.descriptions }} </h2>
  </div>
</template>
<script>
export default {
  props: ['parks']
}
</script>
<style>
</style>
