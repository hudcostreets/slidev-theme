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
  font-size: 3.8em;
  font-weight: 600;
  line-height: 1.1;
  letter-spacing: -0.01em;
  margin-bottom: 0.4em;
}

.section :deep(h1 + p) {
  font-size: 1.3em;
  opacity: 0.85;
  margin: 0.2em 0;
}
</style>
