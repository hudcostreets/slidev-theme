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
  font-size: 5em;
  font-weight: 700;
  line-height: 1.05;
  letter-spacing: -0.02em;
  margin-bottom: 0.5em;
}

.cover :deep(h1 + p),
.cover :deep(.subtitle) {
  font-size: 1.4em;
  font-weight: 400;
  opacity: 0.9;
  margin-top: 0;
}

.cover :deep(p) {
  margin: 0.3em 0;
  font-size: 1.2em;
}
</style>
