<template>
  <Layout>

    <template #doc-before>
      <Breadcrumb />
    </template>

    <template #layout-bottom>
      <FloatingPlayer />
    </template>

    <template #nav-bar-content-after>
      <button class="MascotButton" @click="showMascot = !showMascot">
        Qchan
      </button>
    </template>

    <template #aside-top>
      <div class="BannerSidebar">
        <div class="WindowTitle">
          ♥ Recommended Sites
        </div>

        <div class="BannerList">
          <a v-for="b in banners" :key="b.img" :href="b.link" target="_blank" rel="noopener noreferrer">
            <img :src="b.img" />
          </a>
        </div>
      </div>

      <div class="MascotSidebar">
        <div class="WindowTitle">
          電波人！-dempanchu☆-
        </div>
        <img src="/assets/mascots/denpa.png" alt="Denpa song mascot" class="MascotImage" />
      </div>
    </template>

  </Layout>

  <Mascot v-if="mounted && showMascot" />

</template>

<script setup>

const banners = [
  { img: '/assets/banners/banner.png', link: 'https://denpa.aishitei.ru/about/' },
  { img: '/assets/banners/denpanosekai.jpg', link: 'https://web.archive.org/web/20170803110634/http://www.denpanosekai.com/' },
  { img: '/assets/banners/akibablog.webp', link: 'https://akibablog.blog.jp/' },
  { img: '/assets/banners/loli-aishiteiru.png', link: 'https://loli.aishitei.ru/' },
  { img: '/assets/banners/vnclub.png', link: 'https://vnclub.org/' },
]

import DefaultTheme from 'vitepress/theme'
import { useData, useRouter } from 'vitepress'
import { watch, onMounted, onBeforeUnmount, nextTick, ref } from 'vue'
import Breadcrumb from './Breadcrumb.vue'
import FloatingPlayer from './FloatingPlayer.vue'
import Mascot from './Mascot.vue'

const { Layout } = DefaultTheme
const { page } = useData()
const router = useRouter()
const isNavigating = ref(false)
const mounted = ref(false)
const showMascot = ref(false)

let scrollListener = null
let titleElement = null
let sidebarElement = null
let observer = null
let savedScrollPosition = 0
let fadeTimeout = null
let currentOpacity = 1

watch(showMascot, value => {
  if (mounted.value) {
    localStorage.setItem('show-mascot', value)
  }
})

function setCategoryClass(path) {
  if (typeof document === 'undefined') return

  document.body.classList.remove(
    'category-about',
    'category-reference',
    'category-media',
    'category-bodies',
    'category-language',
    'category-networking',
    'category-misc'
  )

  const isSubpage = (section) =>
    path?.startsWith(`${section}/`) &&
    path !== `${section}/index.md`

  if (isSubpage('about')) {
    document.body.classList.add('category-about')
  } else if (isSubpage('reference')) {
    document.body.classList.add('category-reference')
  } else if (isSubpage('bodies')) {
    document.body.classList.add('category-bodies')
  } else if (isSubpage('language')) {
    document.body.classList.add('category-language')
  } else if (isSubpage('networking')) {
    document.body.classList.add('category-networking')
  } else if (isSubpage('misc')) {
    document.body.classList.add('category-misc')
  }
}

function handleSidebarScroll() {
  if (!sidebarElement || !titleElement) return
  if (isNavigating.value) return

  const scrollTop = sidebarElement.scrollTop
  const fadeStart = 0
  const fadeEnd = 80

  const progress = Math.min(1, (scrollTop - fadeStart) / (fadeEnd - fadeStart))
  const opacity = Math.max(0, 1 - progress)

  currentOpacity = opacity

  titleElement.style.opacity = opacity
  titleElement.style.transform = `translateY(${progress * -8}px)`
}

function saveSidebarPosition() {
  const sidebar = document.querySelector('.VPSidebar')
  if (sidebar) {
    savedScrollPosition = sidebar.scrollTop
    sessionStorage.setItem('sidebar-scroll', savedScrollPosition.toString())
  }
}

function restoreSidebarPosition() {
  const saved = sessionStorage.getItem('sidebar-scroll')
  if (!saved) return

  const restore = () => {
    const sidebar = document.querySelector('.VPSidebar')
    if (sidebar) {
      sidebar.scrollTop = Number(saved)
    }
  }

  requestAnimationFrame(() => {
    restore()
    setTimeout(() => {
      const sidebar = document.querySelector('.VPSidebar')
      if (sidebar && sidebar.scrollTop !== Number(saved)) {
        sidebar.scrollTop = Number(saved)
      }
    }, 100)
  })
}

function setupScrollListener() {
  function isMobile() {
    return window.matchMedia('(max-width: 768px)').matches
  }

  if (isMobile()) return

  if (scrollListener && sidebarElement?.isConnected) {
    sidebarElement.removeEventListener('scroll', scrollListener)
  }

  sidebarElement = document.querySelector('.VPSidebar')
  titleElement = document.querySelector('.VPNavBarTitle')

  if (sidebarElement && titleElement) {
    scrollListener = () => {
      handleSidebarScroll()
      saveSidebarPosition()
    }
    sidebarElement.addEventListener('scroll', scrollListener)
    handleSidebarScroll()
  }
}

function preserveTitleState() {
  titleElement = document.querySelector('.VPNavBarTitle')
  if (titleElement) {
    titleElement.style.transition = 'none'
    titleElement.style.opacity = currentOpacity
    const progress = 1 - currentOpacity
    titleElement.style.transform = `translateY(${progress * -8}px)`

    void titleElement.offsetHeight

    requestAnimationFrame(() => {
      if (titleElement) {
        titleElement.style.transition = 'opacity 0.15s ease, transform 0.15s ease'
      }
    })
  }
}

watch(() => router.route.path, () => {
  isNavigating.value = true
  saveSidebarPosition()

  nextTick(() => {
    preserveTitleState()
    setupScrollListener()

    const path = page.value.relativePath
    setCategoryClass(path)

    nextTick(() => {
      restoreSidebarPosition()

      setTimeout(() => {
        isNavigating.value = false
        handleSidebarScroll()
      }, 100)
    })
  })
})

watch(() => page.value.relativePath, (newPath, oldPath) => {
  if (oldPath === undefined) return

  isNavigating.value = true
  saveSidebarPosition()

  nextTick(() => {
    preserveTitleState()
    setupScrollListener()
    setCategoryClass(newPath)

    nextTick(() => {
      restoreSidebarPosition()
      setTimeout(() => {
        isNavigating.value = false
        handleSidebarScroll()
      }, 100)
    })
  })
})

onMounted(() => {
  if (typeof window !== 'undefined') {
    window.history.scrollRestoration = 'manual'
  }

  mounted.value = true
  showMascot.value = localStorage.getItem('show-mascot') === 'true'

  const path = page.value.relativePath

  setCategoryClass(path)
  setupScrollListener()

  setTimeout(() => {
    restoreSidebarPosition()
    handleSidebarScroll()
  }, 100)

  observer = new MutationObserver(() => {
    const sidebar = document.querySelector('.VPSidebar')
    if (sidebar && sidebar !== sidebarElement) {
      setupScrollListener()
    }
  })

  observer.observe(document.body, {
    childList: true,
    subtree: true
  })
})

onBeforeUnmount(() => {
  if (scrollListener && sidebarElement?.isConnected) {
    sidebarElement.removeEventListener('scroll', scrollListener)
  }
  if (observer) {
    observer.disconnect()
  }
  if (fadeTimeout) {
    cancelAnimationFrame(fadeTimeout)
  }
})

</script>