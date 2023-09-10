<script setup lang="ts">
import ReplaceIp from '../components/ReplaceIp.vue'
import LookUpIp from '@/components/LookUpIp.vue'
</script>

<template>
  <div class="container">
    <ReplaceIp />
    <LookUpIp />
  </div>
</template>

<style scoped>
.container {
  display: flex;
}
</style>
