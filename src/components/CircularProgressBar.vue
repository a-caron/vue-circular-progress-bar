<script setup lang="ts">

  import { computed } from 'vue'

  const props = defineProps<{
    progress: number
  }>()

  // to do : color depending on the value
  const color = 'red'

  const {
    circleDiam,
    circleRadiusMinusBorder,
    circleOffset,
    strokeDashArray,
    strokeDashOffset
  } = useCircle()

  function useCircle() {

    const size = 80
    const border = 6
    const sizeMinusBorder = size - border
    const strokeDashArray = 2 * Math.PI * (sizeMinusBorder / 2)

    const strokeDashOffset = computed(() => {
      return strokeDashArray - (strokeDashArray * props.progress / 100)
    })

    return {
      circleDiam: `${size}px`,
      circleRadiusMinusBorder: sizeMinusBorder / 2,
      circleOffset: `${border / 2}px`,
      strokeDashArray,
      strokeDashOffset
    }
  }
</script>

<template>
  <div class="circular-progress-bar">

    <div
      class="circular-progress-bar__label"
      v-text="progress"/>

    <svg>
      <circle
        v-for="type in ['total', 'progress']"
        :key="type"
        :data-type="type"
        :cx="circleRadiusMinusBorder"
        :cy="circleRadiusMinusBorder"
        :r="circleRadiusMinusBorder"/>
    </svg>
  </div>
</template>

<style scoped lang="scss">

  .circular-progress-bar {
    aspect-ratio: 1/1;
    position: relative;
    width: v-bind(circleDiam);
    height: v-bind(circleDiam);
    background-color: rgba(white, .9);
    padding: 20px;
    border-radius: 14px;

    &__label, svg {
      position: absolute;
      left: 50%;
      top: 50%;
    }

    &__label {
      transform: translate(-50%, -50%);
      font-size: 22px;
      letter-spacing: .02em;
      font-weight: 500;
      color: v-bind(color);
    }

    svg {
      width: v-bind(circleDiam);
      height: v-bind(circleDiam);
      transform: translate(-50%, -50%) rotate(-90deg);

      circle {
        fill: transparent;
        transform: translate(v-bind(circleOffset), v-bind(circleOffset));
      }

      [data-type='total'] {
        stroke-width: 1;
        stroke: v-bind(color);
        opacity: 0.2;
      }

      [data-type='progress'] {
        stroke-width: 3;
        stroke-dasharray: v-bind(strokeDashArray);
        stroke-dashoffset: v-bind(strokeDashOffset);
        stroke: v-bind(color);
      }
    }
  }
</style>