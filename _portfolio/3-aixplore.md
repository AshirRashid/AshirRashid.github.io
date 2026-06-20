---
title: "AiXplore"
excerpt: "Multi-agent MENA travel planning assistant built at a hackathon on the aiXplain platform. Five-agent pipeline: input parsing, restaurant search, attraction search, itinerary generation, and validation with Arabic output.<br/>"
collection: portfolio
---

## The project

Hackathon project. Multi-agent travel assistant for the MENA region, built on the aiXplain platform. You describe a trip in plain language (Arabic included): budget, time limit, current location, preferences. Three complete itinerary options come back with restaurants, attractions, commute times, and taxi fare estimates for each stop.

## Architecture

Five agents in sequence:

**Input parser.** Pulls budget, time limit, current location, final destination, meal type, and food and attraction preferences out of natural language. Arabic input goes through Microsoft Translation first.

**Restaurant search.** Finds restaurants near the current location via Tavily and Google Maps, at least 10 options. Each comes back with coordinates, distance, commute time, price range per person, rating, and a dish recommendation from the reviews.

**Attraction search.** Same structure: at least 10 options with coordinates, entry price, distance, and taxi fare estimates for the country.

**Itinerary generator.** Takes the constraints and both search outputs and builds three plans. Each plan stays within the time and budget, accounts for commute and taxi cost between stops, and ends at the final destination.

**Itinerary inspector.** Reviews each plan for accuracy, re-checks taxi fares and commute times against Maps data, and translates the output to Arabic.

## Key design decisions

**Sequential over parallel.** Running the search agents in parallel was tested. The handoffs were messier than expected at coordination time, so the sequential pipeline stuck. The itinerary generator got cleaner inputs and the output was more predictable.

**Inspector as a separate agent.** Asking one agent to generate and then validate its own itinerary doesn't work well: it tends to rationalize its own errors rather than catch them. A separate inspector reviewed the plans cold and corrected taxi fares and timing without that bias.

**Google Maps for ground truth.** Tavily gives approximate distances. The custom utility model wraps the Maps API as a callable tool so agents get real driving times, distances, and place details instead of guessing.

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
