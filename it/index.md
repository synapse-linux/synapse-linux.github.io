---
layout: default
title: Home
permalink: /it/
lang: it
---

<div class="hero">
  <span class="hero-icon">⬡</span>
  <h1>Synapse Linux</h1>
  <p>Una distribuzione Arch Linux con supporto <strong>AMD Strix Halo AI</strong> — kernel ottimizzati, controllo ventole custom e un installer intelligente.</p>
  <div class="hero-actions">
    <a href="#" class="btn btn-primary">Scarica ISO</a>
    <a href="/it/about/" class="btn btn-secondary">Scopri di più</a>
  </div>
</div>

<section>
  <h2>Caratteristiche</h2>
  <div class="features">
    <div class="card">
      <h3>⚡ Performance Ottimizzate</h3>
      <p>Kernel CachyOS con auto-tuning, BBR e ottimizzazioni CPU per massimo throughput.</p>
    </div>
    <div class="card">
      <h3>🤖 AMD Strix Halo AI</h3>
      <p>Package chooser AI integrato in Calamares — installa <code>synapse-strixhalo-config</code> e controllo ventole per MiniPC AXB35.</p>
    </div>
    <div class="card">
      <h3>📦 Pacchetti Custom</h3>
      <p>PKGBUILD mantenuti per kernel module, impostazioni di sistema e gaming tramite il repository <code>[synapse-linux]</code> pacman.</p>
    </div>
    <div class="card">
      <h3>🔓 Open Source</h3>
      <p>100% libero e open-source. Tutti i PKGBUILD, il builder ISO e i moduli Calamares sono su GitHub.</p>
    </div>
  </div>
</section>

<section>
  <h2>Avvio Rapido</h2>
  <span class="badge">ISO</span>
  <pre><code># Scarica l'ultima ISO
wget https://github.com/synapse-linux/releases/latest/cachyos-desktop-linux-260626.iso

# Verifica il checksum
sha256sum -c cachyos-desktop-linux-260626.iso.sha256

# Scrivi su USB (sostituisci /dev/sdX con il tuo dispositivo)
dd if=cachyos-desktop-linux-260626.iso of=/dev/sdX bs=4M status=progress
</code></pre>
</section>

<section>
  <h2>Ultimi dal Blog</h2>
  <ul class="post-list">
    {% for post in site.posts limit:3 %}
    <li>
      <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <span class="post-meta">{{ post.date | date: '%B %d, %Y' }}</span>
    </li>
    {% endfor %}
  </ul>
  <p style="margin-top: 1rem;"><a href="/it/blog/" class="btn btn-secondary">Leggi tutti gli articoli →</a></p>
</section>
