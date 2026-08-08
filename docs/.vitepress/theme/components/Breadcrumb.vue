<script setup>
import { computed } from 'vue'
import { useData, useRoute } from 'vitepress'

const route = useRoute()
const { theme } = useData()

const sidebarConfig = computed(() => theme.value.sidebar || [])

// Extract icon class from HTML text
const extractIconFromText = (text) => {
  if (!text) return null
  const match = text.match(/class="([^"]*i-lucide:[^"]*)"/)
  return match ? match[1] : null
}

// Clean text by removing HTML tags
const cleanText = (text) => {
  if (!text) return ''
  return text.replace(/<[^>]*>/g, '').trim()
}

const normalizePath = (path) => {
  if (!path) return path
  return path.replace(/\.html$/, '').replace(/\/$/, '')
}

const buildBreadcrumbs = (items, currentPath, breadcrumbPath = []) => {
  const normalizedCurrent = normalizePath(currentPath)

  for (const item of items) {
    const itemLink = item.link
    if (!itemLink) continue

    const normalizedItem = normalizePath(itemLink)

    // Exact match
    if (normalizedItem === normalizedCurrent) {
      return [
        ...breadcrumbPath,
        {
          text: cleanText(item.text),
          link: item.link,
          icon: extractIconFromText(item.text)
        }
      ]
    }

    // Parent detection
    if (normalizedCurrent.startsWith(normalizedItem + '/') && normalizedItem !== normalizedCurrent) {
      if (item.items && item.items.length > 0) {
        const nestedBreadcrumbs = buildBreadcrumbs(
          item.items,
          currentPath,
          [
            ...breadcrumbPath,
            {
              text: cleanText(item.text),
              link: item.link,
              icon: extractIconFromText(item.text)
            }
          ]
        )
        if (nestedBreadcrumbs.length > 0) {
          return nestedBreadcrumbs
        }
      }
    }

    // Search children
    if (item.items && item.items.length > 0) {
      const nestedBreadcrumbs = buildBreadcrumbs(item.items, currentPath, breadcrumbPath)
      if (nestedBreadcrumbs.length > 0) {
        return nestedBreadcrumbs
      }
    }
  }
  return []
}

const breadcrumbs = computed(() => {
  const currentPath = route.path
  return buildBreadcrumbs(sidebarConfig.value, currentPath)
})
</script>

<template>
  <nav v-if="breadcrumbs.length > 0 || $slots.end" class="breadcrumb" aria-label="Breadcrumb">
    <ol class="breadcrumb-list">
      <!-- Home icon -->
      <li class="breadcrumb-home-item">
        <a href="/" class="breadcrumb-home">
          <span class="i-lucide:home w-4 h-4"></span>
        </a>
      </li>

      <!-- Breadcrumb items -->
      <li v-for="(crumb, index) in breadcrumbs" :key="crumb.link || index" class="breadcrumb-item">
        <span class="breadcrumb-separator">
          <span class="i-lucide:chevron-right w-4 h-4"></span>
        </span>
        <a v-if="index !== breadcrumbs.length - 1" :href="crumb.link" class="breadcrumb-link">
          <span v-if="crumb.icon" :class="[crumb.icon, 'breadcrumb-icon']"></span>
          <span class="breadcrumb-text" v-html="crumb.text"></span>
        </a>
        <span v-else class="breadcrumb-current">
          <span v-if="crumb.icon" :class="[crumb.icon, 'breadcrumb-icon']"></span>
          <span class="breadcrumb-text" v-html="crumb.text"></span>
        </span>
      </li>

      <!-- Spacer to push end content to the right -->
      <li class="breadcrumb-spacer"></li>

      <!-- End content slot - wrapped in a div to ensure proper rendering -->
      <li class="breadcrumb-end-content">
        <div class="breadcrumb-end-wrapper">
          <slot name="end"></slot>
        </div>
      </li>
    </ol>
  </nav>
</template>

<style scoped>
.breadcrumb {
  margin-bottom: 1.0rem;
  font-size: 0.875rem;
}

.breadcrumb-list {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 0.5rem;
  list-style: none;
  padding: 0;
  margin: 0;
}

.breadcrumb-home-item,
.breadcrumb-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.breadcrumb-home {
  display: inline-flex;
  align-items: center;
  color: var(--vp-c-text-2);
  transition: color 0.2s ease;
  text-decoration: none;
}

.breadcrumb-home:hover {
  color: var(--vp-c-brand-1);
}

.breadcrumb-link,
.breadcrumb-current {
  display: inline-flex;
  align-items: center;
  gap: 0.375rem;
}

.breadcrumb-link {
  color: var(--vp-c-text-2);
  text-decoration: none;
  transition: color 0.2s ease;
}

.breadcrumb-link:hover {
  color: var(--vp-c-brand-1);
}

.breadcrumb-current {
  font-weight: 500;
  color: var(--vp-c-text-1);
}

.breadcrumb-separator {
  display: inline-flex;
  align-items: center;
  color: var(--vp-c-text-3);
}

.breadcrumb-icon {
  width: 1rem;
  height: 1rem;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.breadcrumb-text {
  display: inline-block;
}

.breadcrumb-home span {
  width: 1rem;
  height: 1rem;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.breadcrumb-separator span {
  width: 1rem;
  height: 1rem;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.breadcrumb-spacer {
  flex: 1;
}

.breadcrumb-end-content {
  display: flex;
  align-items: center;
  margin-left: auto;
}

.breadcrumb-end-wrapper {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  width: 100px;
}

/* Deep styles to properly style the switch inside the breadcrumb */
.breadcrumb-end-wrapper :deep(.VPSwitch) {
  display: inline-flex;
  align-items: center;
}

.breadcrumb-end-wrapper :deep(.VPSwitchAppearance) {
  display: inline-flex;
  align-items: center;
}

.breadcrumb-end-wrapper :deep(.MascotButton) {
  margin: 0 !important;
}

/* Fix for the appearance switch - make it display inline */
.breadcrumb-end-wrapper :deep(.VPSwitch) {
  vertical-align: middle;
}

.breadcrumb-end-wrapper :deep(.VPSwitch .VPSwitchAppearance) {
  display: inline-flex;
}
</style>