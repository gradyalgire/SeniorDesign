# Veer Project Constraints 

Team members: Grady Algire, Brady Cooper, Briar Elliot, Dominic Rowland, Aiden Ward

# Economic 

Veer is funded out of the team's own pockets. This pushes us toward free-tier hosted inference plus the Google Maps Platform for free credit for routing and place search. It also means Veer runs on the driver's existing phone rather than any purpose-built in-dash hardware. 

# Security 

Veer will listen continuously while the car is moving, so the microphone stream is the highest-risk asset in the system. We will not retain raw audio: an utterance is transcribed, converted into a routing constraint such as "avoid highways," and discarded, with only that constraint held for the length of the trip. Passengers are not users of the app and have not agreed to anything, so a visible listening indicator and a one-tap mute are required before any in-car demo. On-device transcription would keep audio off third-party servers entirely. 

# Legal 

Ohio's distracted driving law, ORC 4511.204, makes handling a phone a primary offense while permitting voice-operated use, which is the legal basis for Veer's voice-first interface and eliminates any feature that asks the driver to tap the screen in motion. Google Maps Platform terms also prohibit caching most place data beyond 30 days and displaying its results over a competing base map, so our caching layer expires on a 30-day timer and we cannot swap in a cheaper map tile provider later. 

# Ethical 

Every response competes with the road for the driver's attention, so we aim to keep replies at one short sentence and push anything longer to the screen for a passenger or a stop. Interpreting "I'm hungry for [restaurant name]" also puts us in the business of choosing which business a driver visits, so results are ranked only by stated preference and detour cost.