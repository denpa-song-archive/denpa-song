---
layout: home
title: "Denpa Song Archive"
description: "Denpa Song Archive to please the heart of otaku!"
---

<div class="site-title">
  <a href="https://github.com/denpa-song-archive/denpa-song" target="_blank" rel="noopener noreferrer">denpa.aishitei.ru</a>
</div>

<div class="home-header">
  <div class="home-icon-container">
    <img src="/assets/mascots/denpa_alt.png" alt="Denpa Song" class="home-icon" />
  </div>
  <div class="denpa-quote">
    <p class="quote-source">documenting & preserving denpa song culture!<br>
    <a href="https://discord.gg/BFmCKFvMzA">join our discord to contribute & banter!</a></p>
  </div>
</div>

<div class="introduction-list">

  <a href="/about/" class="introduction-item">
    <div class="introduction-item-header">
      <span class="i-lucide:badge-info"></span>
      <h3>About</h3>
      <span class="separator">|</span>
      <p>What is denpa song?</p>
    </div>
  </a>

  <a href="/reference/" class="introduction-item">
    <div class="introduction-item-header">
      <span class="i-lucide:book-open"></span>
      <h3>Reference</h3>
      <span class="separator">|</span>
      <p>Where is documentation?</p>
    </div>
  </a>

  <a href="/media/" class="introduction-item">
    <div class="introduction-item-header">
      <span class="i-lucide:headphones"></span>
      <h3>Media</h3>
      <span class="separator">|</span>
      <p>Where & what to stream?</p>
    </div>
  </a>

  <a href="/bodies/" class="introduction-item">
    <div class="introduction-item-header">
      <span class="i-lucide:user"></span>
      <h3>Bodies</h3>
      <span class="separator">|</span>
      <p>Who is part of subculture?</p>
    </div>
  </a>

  <a href="/misc/" class="introduction-item">
    <div class="introduction-item-header">
      <span class="i-lucide:beer"></span>
      <h3>Misc</h3>
      <span class="separator">|</span>
      <p>What else is related?</p>
    </div>
  </a>

</div>

<style>
/* ========== HOME HEADER ========== */
.home-header {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1rem;
  max-width: 600px;
  margin: 0 auto 1rem auto;
  padding: 0 1rem;
}

/* ========== HOME ICON ========== */
.home-icon-container {
  flex-shrink: 0;
}

.home-icon {
  width: 200px;
  height: 200px;
  object-fit: contain;
}

/* ========== QUOTE ========== */
.denpa-quote {
  flex: 1;
  text-align: left;
}

.quote-source {
  font-size: 0.9rem;
  color: var(--vp-c-text-2);
  font-style: italic;
  margin: 0;
  line-height: 1.5;
}

.quote-source a {
  color: var(--vp-c-brand-1);
  text-decoration: none;
  font-size: inherit;
}

.quote-source a:hover {
  text-decoration: underline;
}

/* ========== SITE TITLE ========== */
.site-title {
  text-align: center;
  margin-top: 2rem;
  padding-bottom: 1rem;
  border-bottom: 1px solid var(--vp-c-divider);
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
}

.site-title a {
  font-size: 0.85rem;
  text-decoration: none;
  letter-spacing: 0.1em;
  transition: color 0.2s ease;
  font-family: monospace;
}

.site-title a:hover {
  color: var(--vp-c-brand-1);
}

/* ========== INTRODUCTION LIST ========== */
.introduction-list {
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
  margin: 1rem auto;
  max-width: 600px;
  width: 100%;
}

.introduction-item {
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid var(--vp-c-divider);
  padding: 0.5rem 0.8rem;
  background: var(--vp-c-bg-soft2);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  text-decoration: none;
  color: inherit;
  cursor: pointer;
  border-radius: 0 !important;
  min-height: 2.4rem;
}

.introduction-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, .1);
  border-color: var(--vp-c-brand-1);
}

.introduction-item .introduction-item-header {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.6rem;
  width: auto;
}

.introduction-item span[class*="i-lucide"] {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  color: var(--vp-c-brand-1);
  flex-shrink: 0;
  width: 1.2rem;
  height: 1.2rem;
  font-size: 1.2rem;
}

.introduction-item h3 {
  margin: 0;
  color: var(--vp-c-brand-1);
  border-left: none;
  padding: 0;
  background: transparent;
  font-size: 1rem;
  font-weight: 600;
  flex-shrink: 0;
  line-height: 1;
  text-align: center;
}

.introduction-item .separator {
  color: var(--vp-c-text-3);
  opacity: 0.4;
  flex-shrink: 0;
  font-size: 1rem;
}

.introduction-item p {
  margin: 0;
  font-size: 0.9rem;
  color: var(--vp-c-text-2);
  line-height: 1.2;
  flex-shrink: 0;
  text-align: center;
}

/* ========== RESPONSIVE ========== */
@media (max-width: 600px) {
  .home-header {
    flex-direction: column;
    gap: 0.5rem;
    text-align: center;
  }

  .denpa-quote {
    text-align: center;
  }

  .home-icon {
    width: 60px;
    height: 60px;
  }

  .quote-source {
    font-size: 0.75rem;
  }

  .site-title a {
    font-size: 0.7rem;
  }

  .introduction-list {
    padding: 0 0.75rem;
    gap: 0.5rem;
  }

  .introduction-item {
    padding: 0.4rem 0.6rem;
    min-height: 2.2rem;
  }

  .introduction-item .introduction-item-header {
    gap: 0.4rem;
  }

  .introduction-item span[class*="i-lucide"] {
    width: 1rem;
    height: 1rem;
    font-size: 1rem;
  }

  .introduction-item h3 {
    font-size: 0.9rem;
  }

  .introduction-item p {
    font-size: 0.9rem;
  }

  .introduction-item .separator {
    font-size: 0.9rem;
  }
}
</style>