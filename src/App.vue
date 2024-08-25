<script setup lang="ts">
  import CircularProgressBar from "./components/CircularProgressBar.vue"
  // to do : add randomizer for 3 values
</script>

<template>
  <CircularProgressBar :progress="20" />
  <CircularProgressBar :progress="50" />
  <CircularProgressBar :progress="80" />
</template>

<style>

  body {
    background-color: #242424;
    font-family: ui-sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI Variable Display", "Segoe UI", Helvetica, "Apple Color Emoji", Arial, sans-serif, "Segoe UI Emoji", "Segoe UI Symbol";
  }

  #app {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;

    .circular-progress-bar:not(:first-child) {
      margin-left: 60px;
    }
  }
</style>