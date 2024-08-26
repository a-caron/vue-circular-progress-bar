<script setup lang="ts">

  import { computed } from 'vue'

  const props = defineProps<{
    progress: number
  }>()

  const colors:Record<number, string> = {
    33: '#fa5a3e',
    66: '#28a148',
    100: '#1691d1'
  }

  const color = computed<string>(() => {
    for (const key in colors) {
      const breakpointValue = Number(key)
      if (props.progress <= breakpointValue) {
        return colors[key].toString()
      }
    }
    return colors[100]
  })

  const {
    circleDiamPlusBorder,
    circleRadius,
    circleProgressBorder,
    circleTotalBorder,
    circleOffset,
    circleCirc,
    circleCircProgress
  } = useCircle()

  function useCircle() {

    const diam = 74
    const border = 12
    const radius = diam / 2
    const circ = 2 * Math.PI * radius

    const circProgress = computed(() => {
      return circ - (circ * props.progress / 100)
    })

    return {
      circleDiamPlusBorder: `${diam + border}px`,
      circleRadius: radius,
      circleProgressBorder: border / 2,
      circleTotalBorder: (border / 2) - 2,
      circleOffset: `${border / 2}px`,
      circleCirc: circ,
      circleCircProgress: circProgress
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
        :cx="circleRadius"
        :cy="circleRadius"
        :r="circleRadius"/>
    </svg>
  </div>
</template>

<style scoped lang="scss">

  .circular-progress-bar {
    aspect-ratio: 1/1;
    position: relative;
    width: v-bind(circleDiamPlusBorder);
    height: v-bind(circleDiamPlusBorder);
    background-color: rgba(white, .9);
    padding: 20px;
    border-radius: 14px;

    &__label, svg circle {
      transition: 1.2s;
    }

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
      width: v-bind(circleDiamPlusBorder);
      height: v-bind(circleDiamPlusBorder);
      transform: translate(-50%, -50%) rotate(-90deg);

      circle {
        fill: transparent;
        transform: translate(v-bind(circleOffset), v-bind(circleOffset));
      }

      [data-type='total'] {
        stroke-width: v-bind(circleTotalBorder);
        stroke: v-bind(color);
        opacity: 0.2;
      }

      [data-type='progress'] {
        stroke-width: v-bind(circleProgressBorder);
        stroke-dasharray: v-bind(circleCirc);
        stroke-dashoffset: v-bind(circleCircProgress);
        stroke: v-bind(color);
      }
    }
  }
</style>