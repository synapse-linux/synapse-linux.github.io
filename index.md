---
layout: default
title: Home
---

<div class="hero">
  <span class="hero-icon">⬡</span>
  <h1>Synapse Linux</h1>
  <p>A minimal, fast Linux distribution — built for simplicity and performance.</p>
  <div class="hero-actions">
    <a href="#" class="btn btn-primary">Download ISO</a>
    <a href="/about/" class="btn btn-secondary">Learn More</a>
  </div>
</div>

<section>
  <h2>Features</h2>
  <div class="features">
    <div class="card">
      <h3>⚡ Fast & Lightweight</h3>
      <p>Optimized kernel and minimal resource usage. Boots in seconds, runs on modern hardware.</p>
    </div>
    <div class="card">
      <h3>📦 Simple Package Mgmt</h3>
      <p>Clean package management with rolling releases. Always up-to-date.</p>
    </div>
    <div class="card">
      <h3>🔓 Open Source</h3>
      <p>100% free and open-source. Community-driven development on GitHub.</p>
    </div>
    <div class="card">
      <h3>🌐 Modern Stack</h3>
      <p>Latest kernel, drivers, and userspace. Ready for Wayland, PipeWire, and more.</p>
    </div>
  </div>
</section>

<section>
  <h2>Quick Start</h2>
  <span class="badge">Getting started</span>
  <pre><code># Download the latest ISO
wget https://synapse-linux.github.io/releases/latest.iso

# Write to USB
dd if=latest.iso of=/dev/sdX bs=4M status=progress
</code></pre>
</section>

<section>
  <h2>Latest from the Blog</h2>
  <ul class="post-list">
    {% for post in site.posts limit:3 %}
    <li>
      <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <span class="post-meta">{{ post.date | date: '%B %d, %Y' }}</span>
    </li>
    {% endfor %}
  </ul>
  <p style="margin-top: 1rem;"><a href="/blog/" class="btn btn-secondary">Read all posts →</a></p>
</section>
