<template>
  <div class="slidev-layout default">
    <slot />
  </div>
</template>

<style scoped>
.default {
  display: flex;
  flex-direction: column;
  position: relative;
  overflow: hidden;
}
</style>
