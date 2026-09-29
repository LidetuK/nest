<script setup lang="ts">
import OutlineRail from '../components/OutlineRail.vue'
</script>

<template>
  <div class="slide-shell">
    <OutlineRail />
    <div class="slide-main slide-main--center">
      <div class="slide-center-inner">
        <slot />
      </div>
    </div>
  </div>
</template>
