# Veer — Project Description

**Course:** Computer Science Senior Design I
**Advisor:** Dr. Jillian Aurisano
**Team:** Grady Algire, Brady Cooper, Briar Elliot, Dominic Rowland, Aiden Ward
**Version:** 1.0

---

## Overview

Veer is a navigation service that uses AI to determine a driver's route. Instead of typing a destination and accepting whatever path the app returns, the driver talks to Veer while driving. The system interprets what it hears as routing constraints and preferences, then plans or re-plans the route around them.

A driver who says "I'm hungry for McDonald's" gets a route with a McDonald's stop inserted at a low detour cost. A driver who says "I don't want to drive in the rain" gets a route weighted away from active precipitation. The same interface stays open to additional context sources and features as the project grows.

## The problem

Existing navigation apps optimize for a single objective — usually time — and expect the driver to express everything else by hand. Adding a stop, avoiding a highway, or changing a preference mid-trip means touching the screen, which is both unsafe and illegal to do while driving in Ohio. Real driving decisions are messy and stated in plain language, and no mainstream app accepts them that way.

## What Veer does

Veer converts natural speech into structured routing constraints and applies them to a live route.

1. **Listen.** The app captures speech while the vehicle is in motion.
2. **Interpret.** An utterance is transcribed and converted into a structured constraint (`add_waypoint: McDonald's`, `avoid: precipitation`, `avoid: highways`).
3. **Re-plan.** Active constraints are applied to the route and the driver hears a one-sentence confirmation.
4. **Discard.** Raw audio is not retained. Constraints are held only for the length of the trip.

## Users

The primary user is a driver using a phone mounted in their own vehicle. Passengers are present in the cabin but are not users of the app and have not agreed to anything, which is why a visible listening indicator and a one-tap mute are requirements rather than nice-to-haves.

## Scope

**In scope for the first release**

- Voice capture with an explicit listening indicator and mute
- Transcription and constraint extraction from natural language
- Waypoint insertion by business or category ("find a McDonald's on the way")
- Route preference constraints (avoid highways, avoid tolls, avoid weather)
- Turn-by-turn routing and voice guidance
- Mobile application on the driver's existing phone

**Out of scope for the first release**

- Purpose-built in-dash hardware
- Accounts, saved profiles, or cross-trip learning
- Offline routing
- Any paid-placement or sponsored-result revenue model

**Under consideration for later**

- Additional context sources beyond speech (calendar, traffic incidents, vehicle state)
- Persistent preferences across trips
- Multi-stop trip planning

## High-level architecture

```
Driver speech
    ↓
Voice capture (mobile client)
    ↓
Speech-to-text
    ↓
Constraint extraction (LLM → structured constraints)
    ↓
Routing engine (Google Maps Platform: Directions, Places)
    ↓
Route + one-sentence spoken confirmation → driver
```

Raw audio terminates at the transcription step and is not stored. Only extracted constraints move forward, and they expire at the end of the trip.

## Technology

| Layer | Choice |
| --- | --- |
| Client | TBD |
| Speech-to-text | TBD — cloud transcription over TLS, server-side deletion |
| Constraint extraction | TBD — free-tier hosted inference |
| Routing and places | Google Maps Platform (Directions, Places) |
| Backend | TBD |
| Source control | GitHub |
| Deployment | TBD |

## Constraints that shape the design

- **Economic.** Veer is funded out of the team's own pockets, which pushes us toward free-tier hosted inference and the Google Maps Platform free credit, and keeps the app on the driver's existing phone.
- **Security.** The microphone stream is the highest-risk asset in the system. Raw audio is discarded after transcription; only the extracted constraint persists, and only for the trip.
- **Legal.** Ohio's distracted driving law (ORC 4511.204) makes handling a phone a primary offense while permitting voice-operated use. This is the basis for the voice-first interface and rules out any feature requiring a screen tap in motion. Google Maps Platform terms cap place-data caching at 30 days and prohibit displaying its results over a competing base map.
- **Ethical.** Every response competes with the road for the driver's attention, so spoken replies stay at one short sentence. Because Veer chooses which business a driver visits, results are ranked only by stated preference and detour cost.

## Success criteria

- A driver can state a constraint aloud and see the route change without touching the phone.
- Common constraint phrasings are interpreted correctly at a rate the team measures and reports.
- The full loop — speech to re-planned route — completes fast enough to be useful at driving speed.
- The system demonstrably retains no raw audio.
