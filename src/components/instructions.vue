<template>
  <div id="intro">
    <h1 id="app-title">NC State Parks Road Trip Planner </h1>
    <div id="instructions-container">
    <h2 id="welcome">Welcome! This tool will help you plan the ultimate road trip to visit some of NC's most beautiful places. </h2>
    <p id="instructions"> <span class="color-text">To begin,</span> enter where you'd like to start your trip and where you'd like to end it (City, State OR zip code). Then, <span class="color-text">click on the markers on the map</span> to pull up information about each park.
      You can <span class="color-text">then choose to add/remove the park from your route</span>, and the map will automatically update and optimize the order in which you should visit the parks to save time.
      Your total driving time and distance will be calculated below as well. Have fun and enjoy NC's beautiful parks! </p>
    </div>
  </div>
</template>

<script>
</script>

<style>
#intro {
  padding-top: 20px;
}
#app-title {
  color: #fff;
  font-size: 3em;
  font-weight: bold;
  padding: 10px;
  font-family: 'Neucha', sans-serif;
}
#welcome {
  font-size: 1.4em;
}
#instructions-container {
  background-color: #fff;
  border-radius: 8px;
  margin: 15px;
  color: #000;
  padding: 15px;
}
#instructions {
  margin-top: 6px;
}
</style>
