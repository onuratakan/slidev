<script setup lang="ts">
import { watch } from 'vue'
import { currentRoute, clicks, rawRoutes, nextRoute } from '../logic/nav'
import { getSlideClass } from '../utils'
import SlideWrapper from './SlideWrapper.vue'

// preload next route
watch(currentRoute, () => {
  if (currentRoute.value?.meta && currentRoute.value.meta.preload !== false)
    currentRoute.value.meta.__preloaded = true
  if (nextRoute.value?.meta && nextRoute.value.meta.preload !== false)
    nextRoute.value.meta.__preloaded = true
}, { immediate: true })
</script>

<template>
  <template v-for="route of rawRoutes" :key="route.path">
    <SlideWrapper
      :is="route?.component"
      v-show="route === currentRoute"
      v-if="route.meta?.__preloaded || route === currentRoute"
      :clicks="route === currentRoute ? clicks : 0"
      :clicks-elements="route.meta?.__clicksElements || []"
      :clicks-disabled="false"
      :class="getSlideClass(route)"
    />
  </template>
</template>
