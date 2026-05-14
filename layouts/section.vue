<template>
  <div class="slidev-layout section">
    <div class="section-inner">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.section {
  display: grid;
  place-items: center;
  text-align: center;
  position: relative;
  overflow: hidden;
}

.section :deep(h1) {
  font-size: 2.8em;
  font-weight: 600;
  line-height: 1.15;
  margin-bottom: 0.3em;
}

.section :deep(h1 + p) {
  font-size: 1.1em;
  opacity: 0.85;
  margin: 0.2em 0;
}
</style>
