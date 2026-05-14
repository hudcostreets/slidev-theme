<template>
  <div class="slidev-layout two-cols">
    <div class="header">
      <slot />
    </div>
    <div class="cols">
      <div class="col left">
        <slot name="left" />
      </div>
      <div class="col right">
        <slot name="right" />
      </div>
    </div>
  </div>
</template>

<style scoped>
.two-cols {
  display: flex;
  flex-direction: column;
}

.header :deep(h1) {
  margin-bottom: 0.3em;
}

.cols {
  flex: 1;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2em;
  min-height: 0;
}

.col {
  min-width: 0;
  overflow: hidden;
}
</style>
