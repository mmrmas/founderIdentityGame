# The Founder Identity Game

![CI](https://github.com/mmrmas/founderIdentityGame/actions/workflows/ci.yml/badge.svg)

A Vite app for practicing founder positioning through randomized identity pillars, challenge cards, audience types, constraint cards, timed speaking rounds, and an Anti-BS proof drill.

## How to play

1. Click **New round** to deal a fresh practice set.
2. Use the cards to frame a short founder story for the selected audience.
3. Toggle the **Constraint card** on/off to add or remove a challenge.
4. Press **Start** to begin the timer, speak for the selected duration, and then pause or reset as needed.
5. Rate your round with **Clarity**, **Coherence**, and **Memorability**.
6. Press **Complete round** to save it and immediately move to the next one.

You can also switch to **Anti-BS** mode for a proof-focused exercise: take the claim, make it concrete, and show a real example.

## Run locally

```bash
npm install
npm run dev
```

## Custom deck template

Use `public/deckTemplates/custom-deck-template.txt` to create a tailored set of cards for your own practice. The app also includes a **Download template** button in the custom deck controls.

If you want a personal starter deck, place `public/deckTemplates/sam-deck-template.txt` in the same folder and add it to `.gitignore` so it stays private.

Upload it through the app and reset to return to the default deck.

## Build

```bash
npm run build
```
