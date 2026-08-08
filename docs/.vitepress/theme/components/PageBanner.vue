<template>
    <div class="page-banner">
        <img v-if="image" :src="image" class="banner-image" alt="" />

        <div class="banner-overlay">
            <h1 class="banner-title">
                <slot>{{ title }}</slot>
            </h1>

            <p v-if="subtitle" class="banner-subtitle">
                {{ subtitle }}
            </p>
        </div>
    </div>
</template>

<script setup>
defineProps({
    title: { type: String, default: '' },
    subtitle: { type: String, default: '' },
    image: { type: String, default: '' }
})
</script>

<style scoped>
.page-banner {
    position: relative;
    width: 100%;
    height: 100px;
    margin: 12px 0;
    overflow: hidden;
    border: 2px solid color-mix(in srgb, var(--vp-c-divider) 60%, transparent);
    image-rendering: pixelated;
    background: var(--vp-c-bg-soft2);
}

.banner-image {
    width: 100%;
    height: 100%;
    object-fit: cover;
    filter: saturate(1.25) contrast(1.1) brightness(0.95);
}

.banner-overlay {
    position: absolute;
    inset: 0;
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding: 12px 16px;
    background: linear-gradient(90deg, rgba(10, 5, 20, 0.001), rgba(229, 226, 196, 0.052), rgba(20, 19, 5, 0.013));
}

.banner-title {
    font-size: 18px;
    font-weight: bold;
    margin: 0;
    color: var(--vp-c-brand-1);
    letter-spacing: 0.5px;
}

.banner-subtitle {
    margin-top: 0px;
    font-size: 12px;
    color: var(--vp-c-text-3);
    opacity: 0.9;
}

.page-banner::after {
    content: "";
    position: absolute;
    inset: 0;
    background: repeating-linear-gradient(0deg, rgba(255, 255, 255, 0.04), rgba(255, 255, 255, 0.04) 1px, transparent 2px, transparent 4px);
    opacity: 0.25;
    pointer-events: none;
}

.page-banner::before {
    content: "";
    position: absolute;
    inset: 0;
    pointer-events: none;
}

@media (max-width: 768px) {
    .page-banner {
        height: auto;
    }

    .page-banner .banner-title {
        font-size: 15px !important;
    }

    .banner-image {
        object-fit: contain;
    }

    .banner-overlay {
        padding: 0px 16px;
        justify-content: space-between;
    }

    .banner-subtitle {
        margin-top: -10px;
        font-size: 10px !important;
    }
}
</style>