<template>
  <footer v-if="!isHidden" class="hccs-footer" :class="{ light: isLight }">
    <a v-if="url" class="hccs-foot-url" :href="urlHref" target="_blank">{{ url }}</a>
    <a class="hccs-foot-logo" :href="logoHref" target="_blank">
      <img :src="logoSrc" alt="HCCS" />
    </a>
  </footer>
</template>

<script setup>
import { configs, useNav } from '@slidev/client'
import { computed } from 'vue'

const { currentSlideRoute, currentLayout } = useNav()

// Deck-level `footer:` block in headmatter overrides defaults:
//   footer:
//     url: tower.hccs.dev      # link text shown above logo (omit to hide)
//     urlHref: https://tower.hccs.dev  # defaults to https://<url>
//     logoHref: https://hudcostreets.org
//     hideOn: [cover]          # layouts to hide footer on (default: [])
const deckFooter = computed(() => (configs.footer ?? {}))

const frontmatter = computed(() => currentSlideRoute.value?.meta?.slide?.frontmatter ?? {})
const slideFooter = computed(() => {
  const v = frontmatter.value.footer
  if (v === false) return { hidden: true }
  return (typeof v === 'object' && v !== null) ? v : {}
})

const url = computed(() => slideFooter.value.url ?? deckFooter.value.url ?? null)
const urlHref = computed(() => {
  const explicit = slideFooter.value.urlHref ?? deckFooter.value.urlHref
  if (explicit) return explicit
  return url.value ? `https://${url.value}` : '#'
})
const logoHref = computed(() => slideFooter.value.logoHref ?? deckFooter.value.logoHref ?? 'https://hudcostreets.org')

// `class: light` on a slide flips fg/bg; pick the matching logo so it stays
// readable on either background.
const slideClass = computed(() => frontmatter.value.class ?? '')
const isLight = computed(() => {
  if (/\blight\b/.test(slideClass.value)) return true
  if (/\bdark\b/.test(slideClass.value)) return false
  return (configs.colorSchema ?? 'dark') === 'light'
})

const logoSrc = computed(() => isLight.value ? '/hccs-green.png' : '/hccs-white.png')

const hideOn = computed(() => deckFooter.value.hideOn ?? [])
const isHidden = computed(() =>
  slideFooter.value.hidden === true
  || hideOn.value.includes(currentLayout.value)
  || frontmatter.value.footer === false
)
</script>

<style scoped>
.hccs-footer {
  position: absolute;
  bottom: 0.6rem;
  right: 0.8rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.15rem;
  z-index: 20;
  pointer-events: auto;
  font-family: var(--hccs-font-sans);
  color: var(--hccs-text-on-dark);
}
.hccs-footer.light {
  color: var(--hccs-text-on-light);
}

.hccs-foot-url {
  font-size: 0.7rem;
  border-bottom: 1px dotted currentColor;
  color: inherit;
  text-decoration: none;
  opacity: 0.85;
}
.hccs-foot-url:hover {
  opacity: 1;
}

.hccs-foot-logo {
  display: block;
  line-height: 0;
}
.hccs-foot-logo img {
  width: 3rem;
  height: 3rem;
  display: block;
  object-fit: contain;
}
</style>
