---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

{% if site.author.googlescholar %}
  <div class="wordwrap">You can also find my articles on <a href="{{site.author.googlescholar}}">my Google Scholar profile</a>.</div>
{% endif %}

{% include base_path %}

<style>
.pf-viz { --pf-ink: #1a1c20; --pf-muted: #6b7280; --pf-line: #e6e8ec; --pf-mono: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace; margin: 1em 0; }
.pf-viz .pf-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 14px; }
.pf-viz .pf-grid.solo { grid-template-columns: 1fr; }
.pf-viz .pf-card { display: flex; flex-direction: column; border: 1px solid var(--pf-line); border-radius: 12px; overflow: hidden; background: #fff; color: inherit; transition: box-shadow .15s ease, transform .15s ease, border-color .15s ease; }
.pf-viz .pf-card, .pf-viz .pf-card *, .pf-viz .pf-card:hover, .pf-viz .pf-card:hover * { text-decoration: none !important; }
.pf-viz .pf-card:hover { box-shadow: 0 6px 20px rgba(20,24,40,.09); transform: translateY(-2px); border-color: #cfe0ff; }
.pf-viz .pf-card:focus, .pf-viz .pf-card:active, .pf-viz .pf-card:focus-visible { outline: none; border-color: #cfe0ff; }
.pf-viz .pf-card .strip { height: 6px; background: var(--pf-accent, #2f6df6); }
.pf-viz .pf-card .body { display: flex; flex-direction: column; flex: 1; padding: 13px 15px 15px; }
.pf-viz .pf-card .badge { font-size: 10px; font-weight: 700; letter-spacing: .06em; text-transform: uppercase; color: var(--pf-accent, #2f6df6); }
.pf-viz .pf-card .ct { font-weight: 700; font-size: 15px; color: var(--pf-ink); margin: 4px 0 3px; line-height: 1.3; }
.pf-viz .pf-card .cx { font-size: 12.5px; color: var(--pf-muted); line-height: 1.5; }
.pf-viz .pf-card .go { font-family: var(--pf-mono); font-size: 12px; color: var(--pf-accent, #2f6df6); margin-top: auto; padding-top: 9px; display: inline-block; }
@media (max-width: 820px) { .pf-viz .pf-grid { grid-template-columns: 1fr 1fr; } }
@media (max-width: 620px) { .pf-viz .pf-grid, .pf-viz .pf-grid.solo { grid-template-columns: 1fr; } }
</style>

<div class="pf-viz">
  <div class="pf-grid solo">

    <a class="pf-card" href="{{ base_path }}/publication/2026-03-20-paper-title-number-1" style="--pf-accent:#0f9b8e">
      <div class="strip"></div>
      <div class="body">
        <div class="badge">Accepted · IEEE IJCNN 2026</div>
        <div class="ct">Rethinking Evaluation of Multiple Sclerosis (MS) Lesion Segmentation Models</div>
        <div class="cx">IJCNN SS46: Computationally Intelligent Techniques in Early Prediction and Detection of Brain Disorders. Final manuscript and links to follow once published.</div>
        <div class="go">View publication →</div>
      </div>
    </a>

  </div>
</div>
