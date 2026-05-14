<template>
  <footer v-if="!isHidden" class="hccs-footer" :class="{ light: isLight }">
    <a v-if="url" class="hccs-foot-url" :href="urlHref" target="_blank">{{ displayUrl }}</a>
    <a class="hccs-foot-logo" :href="logoHref" target="_blank">
      <img :src="logoSrc" alt="HCCS" />
    </a>
  </footer>
</template>

<script setup>
import { configs, useNav } from '@slidev/client'
import { computed } from 'vue'

const { currentSlideRoute, currentLayout, currentSlideNo } = useNav()

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

// When deck has `footer.slideNum: true` (or per-slide override), append `/<n>`
// to the displayed URL — so it matches the per-slide QR target. Skipped on
// slide 1 (cover) by default, matching `qr.skipSlides` convention.
const deckQrCfg = computed(() => {
  const v = configs.qr
  return (typeof v === 'object' && v !== null) ? v : {}
})
const qrSkipSlides = computed(() => deckQrCfg.value.skipSlides ?? [])
const slideNumEnabled = computed(() => slideFooter.value.slideNum ?? deckFooter.value.slideNum ?? true)
// Skip the `/n` suffix when (1) the slide is in `qr.skipSlides`, (2) the
// slide's layout is `cover` (canonical for the deck overall — `safe26.hccs.dev`
// reads better than `safe26.hccs.dev/1` on the title slide), or (3) it's slide 1
// (matches the default `qr.skipSlides: [1]` convention even when not set).
const showSlideNum = computed(() =>
  slideNumEnabled.value
  && currentSlideNo.value !== 1
  && currentLayout.value !== 'cover'
  && !qrSkipSlides.value.includes(currentSlideNo.value),
)
const displayUrl = computed(() => {
  if (!url.value) return null
  return showSlideNum.value ? `${url.value}/${currentSlideNo.value}` : url.value
})

const urlHref = computed(() => {
  const explicit = slideFooter.value.urlHref ?? deckFooter.value.urlHref
  const base = explicit || (url.value ? `https://${url.value}` : '#')
  if (showSlideNum.value && base !== '#') {
    return `${base.replace(/\/$/, '')}/${currentSlideNo.value}`
  }
  return base
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
  bottom: 0.5rem;
  right: 0.8rem;
  display: flex;
  flex-direction: column;
  /* URL right-justified with logo's right edge; URL is wider than the logo
   * so its left edge overhangs into the QR area, which is fine. */
  align-items: flex-end;
  gap: 0;
  z-index: 20;
  pointer-events: auto;
  font-family: var(--hccs-font-sans);
  color: var(--hccs-text-on-dark);
}
.hccs-footer.light {
  color: var(--hccs-text-on-light);
}

.hccs-foot-url {
  font-size: 0.65rem;
  border-bottom: 1px dotted currentColor;
  color: inherit;
  text-decoration: none;
  opacity: 0.85;
  letter-spacing: 0.01em;
}
.hccs-foot-url:hover {
  opacity: 1;
}

.hccs-foot-logo {
  display: block;
  line-height: 0;
  /* Small breathing room between URL baseline and logo top. */
  margin-top: 0.1rem;
}
.hccs-foot-logo img {
  width: 4.5rem;
  height: 4.5rem;
  display: block;
  object-fit: contain;
}
</style>
