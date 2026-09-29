<script setup lang="ts">
import OutlineRail from '../components/OutlineRail.vue'
</script>

<template>
  <div class="slide-shell">
    <OutlineRail />
    <div class="slide-main slide-main--section">
      <slot />
    </div>
  </div>
</template>
