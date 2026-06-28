---
layout: default
title: Home
permalink: /
lang: en
---

{% include hero.html %}

<section>
  <h2>Features</h2>
  <div class="features">
    <div class="card">
      <h3>🤖 AMD Strix Halo AI</h3>
      <p>ROCm GPU acceleration and XDNA AI driver packages for AMD Strix Halo (Ryzen AI Max 395/385/370). Integrated AI package chooser in Calamares.</p>
    </div>
    <div class="card">
      <h3>📦 ROCm Compute Stack</h3>
      <p>Installs <code>rocm-hip-sdk</code>, <code>rocm-opencl-sdk</code>, <code>miopen-hip</code>, <code>rocblas</code>, and 20+ ROCm packages for full GPU compute.</p>
    </div>
    <div class="card">
      <h3>🔧 Fan &amp; Power Control</h3>
      <p><code>synapse-ec-su-axb35-linux</code> DKMS kernel module with custom fan curves and power modes for AXB35 MiniPCs.</p>
    </div>
    <div class="card">
      <h3>🔩 XDNA AI Driver</h3>
      <p><code>xrt-plugin-amdxdna</code> — AMD XDNA AI kernel driver for the Strix Halo NPU, enabling on-device AI inference.</p>
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
