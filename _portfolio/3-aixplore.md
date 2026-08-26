---
title: "AiXplore"
excerpt: "Multi-agent MENA travel planning assistant built at a hackathon on the aiXplain platform. Five-agent pipeline: input parsing, restaurant search, attraction search, itinerary generation, and validation with Arabic output.<br/>"
collection: portfolio
---

<style>
.aix-viz { --ax-accent: #c86b3c; --ax-ink: #1a1c20; --ax-muted: #6b7280; --ax-line: #e6e8ec; --ax-card: #f7f8fa; --ax-good: #17a673; --ax-mono: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace; margin: 1.2em 0; }
.aix-viz * { box-sizing: border-box; }
.aix-stats { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; }
.aix-stat { background: var(--ax-card); border: 1px solid var(--ax-line); border-radius: 10px; padding: 14px; }
.aix-stat .n { font-size: 22px; font-weight: 700; color: var(--ax-ink); letter-spacing: -.02em; }
.aix-stat .l { font-size: 12px; color: var(--ax-muted); margin-top: 2px; }
.aix-pipe { display: flex; flex-direction: column; gap: 8px; }
.aix-agent { display: grid; grid-template-columns: 34px 1fr; align-items: start; gap: 12px; background: var(--ax-card); border: 1px solid var(--ax-line); border-radius: 10px; padding: 12px 14px; position: relative; }
.aix-agent .num { width: 30px; height: 30px; border-radius: 8px; background: var(--ax-accent); color: #fff; font-family: var(--ax-mono); font-weight: 700; font-size: 13px; display: flex; align-items: center; justify-content: center; }
.aix-agent .an { font-weight: 700; font-size: 14px; color: var(--ax-ink); }
.aix-agent .ad { font-size: 12.5px; color: var(--ax-muted); line-height: 1.45; margin-top: 3px; }
.aix-agent .io { font-family: var(--ax-mono); font-size: 11px; color: var(--ax-accent); margin-top: 5px; }
.aix-arrow { text-align: center; color: var(--ax-line); font-size: 14px; line-height: 0; margin: -2px 0; }
.aix-arrow span { color: #c2c7cf; }
.aix-split { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 12px; }
.aix-panel { border: 1px solid var(--ax-line); border-radius: 10px; padding: 15px; }
.aix-panel h4 { margin: 0 0 6px; font-size: 14px; color: var(--ax-ink); }
.aix-panel p { font-size: 12.5px; color: var(--ax-muted); line-height: 1.5; margin: 0; }
.aix-chips { display: flex; flex-wrap: wrap; gap: 6px; margin-top: 4px; }
.aix-chip { font-family: var(--ax-mono); font-size: 11.5px; background: var(--ax-card); border: 1px solid var(--ax-line); border-radius: 6px; padding: 3px 8px; color: var(--ax-ink); }
@media (max-width: 620px) { .aix-viz .aix-stats { grid-template-columns: repeat(2,1fr); } .aix-viz .aix-split { grid-template-columns: 1fr; } }
</style>

## The project

Hackathon project. Multi-agent travel assistant for the MENA region, built on the aiXplain platform. You describe a trip in plain language (Arabic included): budget, time limit, current location, preferences. Three complete itinerary options come back with restaurants, attractions, commute times, and taxi fare estimates for each stop.

<div class="aix-viz">
<div class="aix-stats">
  <div class="aix-stat"><div class="n">5</div><div class="l">Agents in sequence</div></div>
  <div class="aix-stat"><div class="n">3</div><div class="l">Complete itinerary options returned</div></div>
  <div class="aix-stat"><div class="n">10+</div><div class="l">Restaurant and attraction options each</div></div>
  <div class="aix-stat"><div class="n">AR / EN</div><div class="l">Arabic input and output supported</div></div>
</div>
</div>

## Architecture

Five agents in sequence:

<div class="aix-viz">
<div class="aix-pipe">
  <div class="aix-agent"><div class="num">1</div><div><div class="an">Input parser</div><div class="ad">Pulls budget, time limit, current location, final destination, meal type, and food and attraction preferences out of natural language.</div><div class="io">Arabic input → Microsoft Translation first</div></div></div>
  <div class="aix-arrow"><span>▾</span></div>
  <div class="aix-agent"><div class="num">2</div><div><div class="an">Restaurant search</div><div class="ad">Finds restaurants near the current location via Tavily and Google Maps.</div><div class="io">10+ options: coords, distance, commute, price/person, rating, dish pick</div></div></div>
  <div class="aix-arrow"><span>▾</span></div>
  <div class="aix-agent"><div class="num">3</div><div><div class="an">Attraction search</div><div class="ad">Same structure as restaurant search, tuned for places to visit.</div><div class="io">10+ options: coords, entry price, distance, taxi fare estimate</div></div></div>
  <div class="aix-arrow"><span>▾</span></div>
  <div class="aix-agent"><div class="num">4</div><div><div class="an">Itinerary generator</div><div class="ad">Takes the constraints and both search outputs and builds plans that stay within time and budget, account for commute and taxi cost, and end at the final destination.</div><div class="io">Builds 3 plans</div></div></div>
  <div class="aix-arrow"><span>▾</span></div>
  <div class="aix-agent"><div class="num">5</div><div><div class="an">Itinerary inspector</div><div class="ad">Reviews each plan for accuracy, re-checks taxi fares and commute times against Maps data, and translates the output.</div><div class="io">Output translated to Arabic</div></div></div>
</div>
</div>

## Key design decisions

<div class="aix-viz">
<div class="aix-split">
  <div class="aix-panel"><h4>Sequential over parallel</h4><p>Parallel search agents were tested. The handoffs got messy at coordination time, so the sequential pipeline stuck: the generator got cleaner inputs and more predictable output.</p></div>
  <div class="aix-panel"><h4>Inspector as its own agent</h4><p>One agent that generates then validates its own itinerary tends to rationalize its errors. A separate inspector reviews the plans cold and fixes taxi fares and timing without that bias.</p></div>
  <div class="aix-panel"><h4>Google Maps for ground truth</h4><p>Tavily gives approximate distances. A custom utility model wraps the Maps API as a callable tool, so agents get real driving times, distances, and place details instead of guessing.</p><div class="aix-chips"><span class="aix-chip">Tavily ≈ approx</span><span class="aix-chip">Maps = ground truth</span></div></div>
</div>
</div>

**Technologies:**
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Python</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">aiXplain</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Google Maps API</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Tavily</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Microsoft Translation</span>

**Concepts / Algorithms:**
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Multi-Agent Orchestration</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">LLM Tool Use</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Agentic Pipelines</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Arabic NLP</span>
