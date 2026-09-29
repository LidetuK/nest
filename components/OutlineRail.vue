<script setup lang="ts">
import { computed, nextTick, onMounted, ref, watch } from 'vue'
import { useNav } from '@slidev/client'

const { currentPage, total, go } = useNav()
const tocEl = ref<HTMLElement | null>(null)
const SCROLL_KEY = 'aspio-se2-nest-outline-scroll'

const progressLabel = computed(() => `${currentPage.value} / ${total.value}`)

/** Fixed outline matching slides.md title order (1:1 with Slidev pages). */
const OUTLINE = [
  'Title',
  'Agenda',
  'Context',
  'What Phase 1 is',
  'Challenge N1',
  'N1 · Problem',
  'N1 · Evidence',
  'N1 · Architecture',
  'N1 · Solution',
  'N1 · Rejected',
  'N1 · Plan',
  'Challenge N2',
  'N2 · Problem',
  'N2 · Evidence',
  'N2 · Architecture',
  'N2 · Solution',
  'N2 · Rejected',
  'N2 · Plan',
  'Challenge N3',
  'N3 · Problem',
  'N3 · Evidence',
  'N3 · Architecture',
  'N3 · Solution',
  'N3 · Rejected',
  'N3 · Plan',
  'Closing',
] as const

const entries = computed(() =>
  OUTLINE.map((title, idx) => ({
    page: idx + 1,
    title,
    active: currentPage.value === idx + 1,
  })),
)

async function navigate(page: number, event?: Event) {
  event?.preventDefault()
  event?.stopPropagation()
  if (page === currentPage.value)
    return
  try {
    await go(page)
  }
  catch {
    window.location.hash = `#${page}`
  }
}

function saveScroll() {
  if (tocEl.value)
    sessionStorage.setItem(SCROLL_KEY, String(tocEl.value.scrollTop))
}

function restoreScroll() {
  const raw = sessionStorage.getItem(SCROLL_KEY)
  if (raw && tocEl.value)
    tocEl.value.scrollTop = Number(raw) || 0
}

onMounted(() => {
  nextTick(restoreScroll)
})

watch(currentPage, () => {
  nextTick(() => {
    restoreScroll()
    const active = tocEl.value?.querySelector('.outline-link.active') as HTMLElement | null
    active?.scrollIntoView({ block: 'nearest' })
  })
})
</script>

<template>
  <aside class="outline-rail" aria-label="Presentation outline" @click.stop @pointerdown.stop>
    <div class="outline-brand">
      <div class="outline-brand-title">SE1 → SE2</div>
      <div class="outline-brand-sub">Phase 1 · Nest</div>
    </div>

    <nav ref="tocEl" class="outline-toc" @scroll="saveScroll">
      <button
        v-for="e in entries"
        :key="e.page"
        type="button"
        class="outline-link"
        :class="{ active: e.active }"
        :aria-current="e.active ? 'page' : undefined"
        @click="navigate(e.page, $event)"
        @pointerdown.stop
      >
        {{ e.title }}
      </button>
    </nav>

    <div class="outline-progress">{{ progressLabel }}</div>
  </aside>
</template>
