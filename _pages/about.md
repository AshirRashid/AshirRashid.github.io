---
permalink: /
title: "👋🏼 Hello there, I'm Ashir!"
excerpt: "Ashir Rashid, Computer Science graduate (NYU Abu Dhabi) working across applied ML, data engineering, and LLM research. Published in medical AI (IEEE IJCNN 2026)."
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---



<style>
.intro-viz { --iv-accent: #2f6df6; --iv-ink: #1a1c20; --iv-muted: #6b7280; --iv-line: #e6e8ec; --iv-card: #f7f8fa; --iv-good: #17a673; --iv-mono: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace; margin: 1.1em 0; }
.intro-viz * { box-sizing: border-box; }
.iv-facts { display: flex; flex-wrap: wrap; gap: 7px; }
.iv-fact { font-size: 12.5px; color: var(--iv-ink); background: var(--iv-card); border: 1px solid var(--iv-line); border-radius: 999px; padding: 5px 12px; display: inline-flex; align-items: center; gap: 6px; }
.iv-fact .k { color: var(--iv-muted); }
.iv-lanes { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
.iv-lane { border: 1px solid var(--iv-line); border-radius: 10px; padding: 15px; background: #fff; }
.iv-lane .lh { display: flex; align-items: center; gap: 8px; font-weight: 700; font-size: 14px; color: var(--iv-ink); margin-bottom: 8px; }
.iv-lane .lh .ic { width: 10px; height: 10px; border-radius: 50%; background: var(--iv-accent); flex: 0 0 auto; }
.iv-lane .chips { display: flex; flex-wrap: wrap; gap: 6px; }
.iv-lane .chip { font-family: var(--iv-mono); font-size: 11.5px; color: var(--iv-ink); background: var(--iv-card); border: 1px solid var(--iv-line); border-radius: 6px; padding: 3px 9px; }
.iv-timeline { display: flex; flex-direction: column; gap: 10px; }
.iv-job { display: grid; grid-template-columns: 12px 1fr; gap: 14px; }
.iv-job .rail { position: relative; }
.iv-job .rail::before { content: ""; position: absolute; left: 4px; top: 4px; bottom: -14px; width: 2px; background: var(--iv-line); }
.iv-job:last-child .rail::before { display: none; }
.iv-job .rail .dot { position: absolute; left: 0; top: 3px; width: 10px; height: 10px; border-radius: 50%; background: var(--iv-accent); }
.iv-job .card { border: 1px solid var(--iv-line); border-radius: 10px; padding: 13px 15px; background: #fff; }
.iv-job .card .role { font-weight: 700; font-size: 14px; color: var(--iv-ink); }
.iv-job .card .meta { font-size: 12px; color: var(--iv-muted); margin-top: 1px; }
.iv-job .card .meta a { color: var(--iv-accent); }
.iv-job .card .desc { font-size: 12.5px; color: var(--iv-muted); line-height: 1.5; margin-top: 7px; }
.iv-os { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; }
.iv-oscard { display: block; border: 1px solid var(--iv-line); border-radius: 10px; padding: 13px; background: #fff; color: inherit; transition: box-shadow .15s ease, transform .15s ease, border-color .15s ease; }
.iv-oscard, .iv-oscard *, .iv-oscard:hover, .iv-oscard:hover * { text-decoration: none !important; }
.iv-oscard:hover { box-shadow: 0 6px 20px rgba(20,24,40,.08); transform: translateY(-2px); border-color: #cfe0ff; }
.iv-oscard:focus, .iv-oscard:active, .iv-oscard:focus-visible { outline: none; border-color: #cfe0ff; }
.iv-oscard .on { font-weight: 700; font-size: 13.5px; color: var(--iv-ink); }
.iv-oscard .od { font-size: 12px; color: var(--iv-muted); line-height: 1.45; margin: 5px 0 8px; }
.iv-oscard .go { color: var(--iv-accent); font-family: var(--iv-mono); font-size: 12px; display: inline-block; }
.iv-feats { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
.iv-feat { display: block; border: 1px solid var(--iv-line); border-radius: 12px; overflow: hidden; background: #fff; text-decoration: none; color: inherit; transition: box-shadow .15s ease, transform .15s ease, border-color .15s ease; }
.iv-feat, .iv-feat *, .iv-feat:hover, .iv-feat:hover * { text-decoration: none !important; }
.iv-feat:hover { box-shadow: 0 6px 20px rgba(20,24,40,.08); transform: translateY(-2px); border-color: #cfe0ff; }
.iv-feat .thumb { height: 6px; background: var(--iv-accent); }
.iv-feat.paper .thumb { background: var(--iv-good); }
.iv-feat .body { padding: 12px 14px; }
.iv-feat .badge { font-size: 10px; font-weight: 700; letter-spacing: .06em; text-transform: uppercase; color: var(--iv-accent); }
.iv-feat.paper .badge { color: var(--iv-good); }
.iv-feat .ft { font-weight: 700; font-size: 14px; color: var(--iv-ink); margin: 3px 0 2px; line-height: 1.3; }
.iv-feat .fc { font-size: 12px; color: var(--iv-muted); line-height: 1.4; }
.iv-feat .go { font-family: var(--iv-mono); font-size: 12px; color: var(--iv-accent); margin-top: 8px; display: inline-block; }
.iv-video { border: 1px solid var(--iv-line); border-radius: 12px; overflow: hidden; background: #000; max-width: 560px; }
.iv-video .frame { display: block; position: relative; width: 100%; padding-top: 56.25%; background-size: cover; background-position: center; }
.iv-video .frame .play { position: absolute; inset: 0; display: flex; align-items: center; justify-content: center; }
.iv-video .frame .play svg { width: 68px; height: 48px; filter: drop-shadow(0 2px 6px rgba(0,0,0,.5)); transition: transform .15s ease; }
.iv-video .frame:hover .play svg { transform: scale(1.08); }
.iv-video .cap { background: #fff; padding: 10px 14px; font-size: 12.5px; color: var(--iv-muted); border-top: 1px solid var(--iv-line); }
.iv-video .cap a { color: var(--iv-accent); }
.intro-viz .iv-lead { font-size: 15px; line-height: 1.55; color: var(--iv-ink); margin: 0; }
.page__title { font-size: 1.563em; }
.page__content h2 { margin-top: 1em; padding-bottom: 0.3em; }
.intro-viz { margin: 0.7em 0 !important; }
@media (max-width: 620px) { .intro-viz .iv-lanes, .intro-viz .iv-os, .intro-viz .iv-feats { grid-template-columns: 1fr; } }
</style>

<!-- ![Illustration of combining vision and language modalities](/images/image_to_text_vis.png){: .align-right width="300px"} -->
<div class="intro-viz"><p class="iv-lead">I'm a Computer Science &amp; Applied Mathematics graduate of NYU Abu Dhabi (May 2026) and a published researcher in medical AI.</p></div>

<div class="intro-viz">
<div class="iv-facts">
  <span class="iv-fact"><span class="k">Degree</span> CS & Applied Math, NYU Abu Dhabi</span>
  <span class="iv-fact"><span class="k">Grad</span> May 2026</span>
  <span class="iv-fact"><span class="k">Published</span> IEEE IJCNN 2026</span>
  <span class="iv-fact"><span class="k">Focus</span> Applied ML · Data Eng · LLM research</span>
</div>
</div>

## What I work on

<div class="intro-viz">
<div class="iv-lanes">
  <div class="iv-lane">
    <div class="lh"><span class="ic"></span> Applied AI / ML systems / FDE</div>
    <div class="chips"><span class="chip">RAG pipelines</span><span class="chip">Agentic workflows</span><span class="chip">Azure ML deployment</span><span class="chip">Full-stack delivery</span></div>
  </div>
  <div class="iv-lane">
    <div class="lh"><span class="ic"></span> AI evaluation, privacy & efficiency</div>
    <div class="chips"><span class="chip">Model evaluation frameworks</span><span class="chip">LLM quantization</span><span class="chip">Membership-inference privacy</span></div>
  </div>
</div>
</div>

## Featured

<div class="intro-viz">
<div class="iv-feats">
  <a class="iv-feat paper" href="/publication/2026-03-20-paper-title-number-1">
    <div class="thumb"></div>
    <div class="body">
      <div class="badge">Publication · IEEE IJCNN 2026</div>
      <div class="ft">MS Lesion Segmentation Evaluation</div>
      <div class="fc">A lesion-centric framework that exposes failures standard Dice metrics hide.</div>
      <span class="go">Read the paper →</span>
    </div>
  </a>
  <a class="iv-feat" href="https://ashirrashid.github.io/portfolio/">
    <div class="thumb"></div>
    <div class="body">
      <div class="badge">Portfolio</div>
      <div class="ft">Personal Projects</div>
      <div class="fc">RAG pipelines, agentic systems, LLM quantization, and evaluation work.</div>
      <span class="go">Browse projects →</span>
    </div>
  </a>
</div>
</div>

<!-- 📽️ I actively learn, make personal projects, and iterate on prototypes to test my ideas. -->

<!-- # Selected Experience -->

## Professional Experience

<div class="intro-viz">
<div class="iv-timeline">
  <div class="iv-job"><div class="rail"><span class="dot"></span></div><div class="card">
    <div class="role">Data Scientist</div>
    <div class="meta"><a href="https://gamalearn.com/">GamaLearn</a> · May 2025 to May 2026</div>
    <div class="desc">Designed, experimented with, and deployed data-driven solutions tailored to business needs.</div>
  </div></div>
  <div class="iv-job"><div class="rail"><span class="dot"></span></div><div class="card">
    <div class="role">Deep Learning Researcher</div>
    <div class="meta">NYU Abu Dhabi <a href="https://ebrain4everyone.com/">eBrain Lab</a>, under Prof. Muhammad Shafique</div>
    <div class="desc">Built end-to-end data pipelines for a mixed-precision quantization framework, enabling efficient experimentation and clear interpretation of results, then used the pipeline to optimize frameworks for quantizing and pruning machine learning models.</div>
  </div></div>
  <div class="iv-job"><div class="rail"><span class="dot"></span></div><div class="card">
    <div class="role">Full-Stack Web Developer</div>
    <div class="meta"><a href="https://www.gantom.com/">Gantom Lighting & Controls</a></div>
    <div class="desc">FastAPI and Django on the back-end, GitHub Actions for CI/CD, Firestore and PostgreSQL for storage, Google Cloud Platform and Docker for deployment, React on the front-end. Collaborated across technical, design, and customer support teams.</div>
  </div></div>
</div>
</div>

<!--
## 📜 Reimplementing and Reproducing Papers
Papers reimplemented and results reproduced:
quantization papers
adaptive rag
-->

## Open Source

<div class="intro-viz">
<div class="iv-os">
  <a class="iv-oscard" href="https://github.com/GDQuest/learn-gdscript/pulls?q=is%3Apr+is%3Aclosed+author%3AAshirRashid">
    <div class="on">Learn GDScript</div>
    <div class="od">Free, open-source app for beginners to learn programming with Godot's GDScript language.</div>
    <div class="go">my PRs →</div>
  </a>
  <a class="iv-oscard" href="https://github.com/intelowlproject/IntelOwl/pulls?q=is%3Apr+is%3Aclosed+author%3AAshirRashid">
    <div class="on">IntelOwl</div>
    <div class="od">Open-source OSINT solution for obtaining threat-intelligence data.</div>
    <div class="go">my PRs →</div>
  </a>
  <a class="iv-oscard" href="https://github.com/AshirRashid/HAQ-extension">
    <div class="on">HAQ</div>
    <div class="od">Mixed-precision quantization framework. I maintain an extended fork.</div>
    <div class="go">my fork →</div>
  </a>
</div>
</div>

## Teaching and Community Contributions
<div class="intro-viz"><p class="iv-lead">I made animations using Python to aid in teaching math/CS concepts, and I taught as a tutor at Schoolhouse.</p></div>

<div class="intro-viz">
<div class="iv-video">
  <a class="frame" href="https://www.youtube.com/watch?v=cJfylP2r8Pk" target="_blank" rel="noopener" style="background-image:url('https://i.ytimg.com/vi/cJfylP2r8Pk/hqdefault.jpg')" aria-label="Play: Generating an Electric Pulse Between two Points on YouTube">
    <span class="play"><svg viewBox="0 0 68 48" aria-hidden="true"><path d="M66.5 7.7c-.8-2.9-2.5-5.2-5.4-6C55.8.3 34 .3 34 .3S12.2.3 6.9 1.7C4 2.5 2.3 4.8 1.5 7.7.1 13 .1 24 .1 24s0 11 1.4 16.3c.8 2.9 2.5 5.2 5.4 6C12.2 47.7 34 47.7 34 47.7s21.8 0 27.1-1.4c2.9-.8 4.6-3.1 5.4-6C67.9 35 67.9 24 67.9 24s0-11-1.4-16.3z" fill="#f00"/><path d="M27 34.5L45 24 27 13.5z" fill="#fff"/></svg></span>
  </a>
  <div class="cap">An electric-pulse effect I built with my Python animation engine. More educational content on my <a href="https://www.youtube.com/@ashirr9184">YouTube channel</a>.</div>
</div>
</div>


<!-- TODO

Add more netsec stuff
Add a call to action: view my latest project
Add personal projects about netsec and arp poisining, splunk, firewalls

future projects that will be useful for my website: nlp
projects that I am interested in too: ml-based ids systems & hate speech detection in roman urdu
update linkedin too and buy premium & then reach out to ppl

in general, I need more informaiton on financial systems, business & corporate law

-->
