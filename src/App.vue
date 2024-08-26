<script setup lang="ts">

  import { ref, onMounted, onUnmounted } from 'vue'
  import CircularProgressBar from "./components/CircularProgressBar.vue"

  const {
    values
  } = useInfiniteRandomValues()

  function useInfiniteRandomValues() {

    let intervalId:number
    const values = ref([0, 0, 0])

    onMounted(() => {

      const random100 = () => Math.round(Math.random() * 100)
      const setValues = () => values.value = values.value.map(() => random100())

      setTimeout(setValues)
      intervalId = setInterval(() => setValues(), 3500)
    })

    onUnmounted(() => clearInterval(intervalId))

    return {
      values
    }
  }
</script>

<template>
  <CircularProgressBar :progress="values[0]" />
  <CircularProgressBar :progress="values[1]" />
  <CircularProgressBar :progress="values[2]" />
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