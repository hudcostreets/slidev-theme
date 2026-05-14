<template>
  <div class="slidev-layout cover">
    <div class="cover-inner">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.cover {
  display: grid;
  place-items: center;
  text-align: center;
  position: relative;
  overflow: hidden;
}

.cover-inner {
  max-width: 80%;
}

.cover :deep(h1) {
  font-size: 3.2em;
  font-weight: 700;
  line-height: 1.1;
  letter-spacing: -0.015em;
  margin-bottom: 0.4em;
}

.cover :deep(h1 + p),
.cover :deep(.subtitle) {
  font-size: 1.15em;
  font-weight: 400;
  opacity: 0.85;
  margin-top: 0;
}

.cover :deep(p) {
  margin: 0.25em 0;
}
</style>
