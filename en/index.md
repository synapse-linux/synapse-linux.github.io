---
layout: default
title: Home
permalink: /en/
lang: en
---

{% include hero.html %}

<section>
  <h2>Features</h2>
  <div class="features">
    <div class="card">
      <h3>⚡ Optimized Performance</h3>
      <p>CachyOS kernels with auto-tuning, BBR, and CPU optimizations for maximum throughput.</p>
    </div>
    <div class="card">
      <h3>🤖 AMD Strix Halo AI</h3>
      <p>Integrated AI package chooser in Calamares — install <code>synapse-strixhalo-config</code> and fan control for AXB35 MiniPCs.</p>
    </div>
    <div class="card">
      <h3>📦 Custom Packages</h3>
      <p>Maintained PKGBUILDs for kernel modules, system settings, and gaming optimizations via the <code>[synapse-linux]</code> pacman repository.</p>
    </div>
    <div class="card">
      <h3>🔓 Open Source</h3>
      <p>100% free and open-source. All PKGBUILDs, ISO builder, and Calamares modules available on GitHub.</p>
    </div>
  </div>
</section>

<section>
  <h2>Quick Start</h2>
  <span class="badge">ISO</span>
  <pre><code># Download the latest ISO
wget https://github.com/synapse-linux/releases/latest/cachyos-desktop-linux-260626.iso

# Verify checksum
sha256sum -c cachyos-desktop-linux-260626.iso.sha256

# Write to USB (replace /dev/sdX with your device)
dd if=cachyos-desktop-linux-260626.iso of=/dev/sdX bs=4M status=progress
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
  <p style="margin-top: 1rem;"><a href="/en/blog/" class="btn btn-secondary">Read all posts →</a></p>
</section>
