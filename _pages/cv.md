---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

<style>
.cv-viz { --cv-ink: #1a1c20; --cv-muted: #6b7280; --cv-line: #e6e8ec; --cv-accent: #2f6df6; --cv-mono: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace; margin: 1em 0; }
.cv-viz .cv-bar { display: flex; align-items: center; justify-content: flex-end; gap: 10px; margin-bottom: 12px; }
.cv-viz .cv-bar a { display: inline-flex; align-items: center; gap: 6px; font-size: 13px; font-weight: 600; padding: 8px 14px; border-radius: 8px; text-decoration: none !important; transition: background .15s ease, box-shadow .15s ease, transform .15s ease; }
.cv-viz .cv-bar a.primary { background: var(--cv-accent); color: #fff; }
.cv-viz .cv-bar a.primary:hover { background: #2559d0; box-shadow: 0 4px 14px rgba(47,109,246,.28); transform: translateY(-1px); }
.cv-viz .cv-bar a.ghost { background: #fff; color: var(--cv-accent); border: 1px solid var(--cv-line); }
.cv-viz .cv-bar a.ghost:hover { border-color: #cfe0ff; box-shadow: 0 4px 14px rgba(20,24,40,.06); transform: translateY(-1px); }
.cv-viz .cv-frame { position: relative; width: 100%; height: 82vh; min-height: 520px; border: 1px solid var(--cv-line); border-radius: 12px; overflow: hidden; background: #f7f8fa; box-shadow: 0 1px 3px rgba(20,24,40,.04); }
.cv-viz .cv-frame iframe { width: 100%; height: 100%; border: 0; }
.cv-viz .cv-fallback { font-size: 12.5px; color: var(--cv-muted); line-height: 1.5; margin-top: 10px; text-align: center; }
.cv-viz .cv-fallback a { color: var(--cv-accent); }
</style>

<div class="cv-viz">
  <div class="cv-bar">
    <a class="ghost" href="{{ base_path }}/files/Ashir Rashid Resume.pdf" target="_blank" rel="noopener">Open in new tab →</a>
    <a class="primary" href="{{ base_path }}/files/Ashir Rashid Resume.pdf" download>Download PDF ↓</a>
  </div>
  <div class="cv-frame">
    <iframe src="{{ base_path }}/files/Ashir Rashid Resume.pdf#view=FitH" title="Ashir Rashid CV" loading="lazy"></iframe>
  </div>
  <p class="cv-fallback">If the preview does not load in your browser, <a href="{{ base_path }}/files/Ashir Rashid Resume.pdf" target="_blank" rel="noopener">open the PDF directly</a>.</p>
</div>
