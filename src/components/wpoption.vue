<template>
  <select :parks="parks" multiple id="waypoints">
    <option v-for='wp in wps' :value="wp.name">{{ wp.name }}</option>
  </select>
</template>
<script>
export default {
  data () {
    return {
    wps: [
        {name: 'Gorges State Park'},
        {name: 'Chimney Rock State Park'},
        {name: 'Hanging Rock State Park'}
        ]
      }
    },
  props: ['parks']
}
</script>

<style>
</style>
