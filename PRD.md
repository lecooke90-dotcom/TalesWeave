# TalesWeave PRD v1.1

**Target audience:** Parents and caregivers of children aged 3 to 8
**Status:** Draft, revised from v1.0

## 1. Summary and vision

**Vision.** TalesWeave gives families personalised, screen-free bedtime stories that children help create. A child picks three icons, and the app tells a calm, original audio story that gently winds down into sleep.

**Problem.** Parents tire of rereading the same books, and pre-recorded audiobooks feel generic. Children enjoy choosing, but bedtime is no time for a bright, stimulating screen.

**Scope decision (v1.1).** TalesWeave is a story app only. Parent voice cloning, the habit tracker and behaviour-reinforcement themes have been removed from the product.

*Note: the v1.0 statistics and blue-light claims have been dropped. Re-add them only with cited sources.*

## 2. Users and use cases

**The tired parent (primary).** Wants a low-effort, calming bedtime routine with no screen to fight over.

**The curious child (3 to 8).** Wants choice and ownership over the story.

**Use cases**

1. **Bedtime story.** Parent and child choose three icons together. A 5 to 10 minute audio story begins within seconds, over a soft ambient soundscape.
2. **Sleep transition.** The story ends and fades into low-volume ambient sound or stops, per the sleep timer.
3. **Replay.** The family saves a favourite story and replays it offline, for example in the car or on holiday.

## 3. Functional requirements

### 3.1 Story builder

Three icon picks, one from each group:

- **Protagonist:** for example Brave Bunny, Cozy Bear, Starlit Dragon.
- **World:** for example Enchanted Forest, Floating Cloud Kingdom, Undersea Castle.
- **Quest:** one of six gentle quests (below).

| # | Quest | Icon | Story seed |
|---|-------|------|------------|
| 1 | Find the Lost Star | Small star with a trail | A star has slipped from the sky. The hero carries it home and the world glows and quiets. |
| 2 | Make a New Friend | Two figures holding hands | The hero meets someone shy and new. They share a quiet moment, yawn and say goodnight. |
| 3 | Follow the Moonbeam | Crescent moon with a silver path | A moonbeam lights a trail that leads to the coziest place to curl up. |
| 4 | Bake Moon Cookies | Cookie with a crescent bite | The hero gathers sleepy ingredients, bakes something warm and shares it. |
| 5 | Help a Sleepy Cloud Home | Fluffy cloud with closed eyes | A drowsy cloud has wandered off course. The hero guides it home and it drifts off. |
| 6 | Plant a Dream Seed | Seed with a tiny sprout | The hero plants a seed and tends it. At nightfall it blooms into a glowing, humming flower. |

**Icon rules.** Icons must differ by silhouette, not only colour, for young children and colour-blind users. Test with real 3 to 4 year olds before art is final.

**Combinations.** 3 protagonists x 3 worlds x 6 quests gives 54 combinations at launch. The prompt handles odd pairings with dreamy logic and never explains them.

**Optional child name.** A parent can add the child's first name or a pet name so it appears in stories. This is off by default and requires parental consent (see section 6).

### 3.2 Story generation

- Slow, rhythmic sentences with repeated, soothing motifs.
- No loud sounds, peril, high-stress conflict or cliffhangers. Any obstacle must be mild (for example, a cloud that is slow).
- The final third of every story is always a wind-down, whatever the quest.
- Content filtering blocks scary, violent or age-inappropriate output before it reaches audio.
- Each story is an original generation, with variety from the 54 combinations plus model variation.

### 3.3 Audio and playback

- **Narrators:** 3 voices at launch, expanding to 4 to 6 later.
- **Soundscapes:** 4 ambient loops at launch (for example soft rain, ocean waves, night crickets, lullaby piano), mixed under narration at about -18 dB.
- **Pacing:** speech slows gradually toward the end. *Technical risk: streaming TTS may not support gradual rate change. Prototype early. Fallback is a slower pace for the final third only.*
- **Sleep timer:** fade out over 5 to 15 minutes after the story, into ambient sound or silence.

### 3.4 Night-mode interface

- Deep amber-on-black theme.
- Screen dims to a static starry-night view once playback starts.
- Controls are large and simple, with grown-up settings (name, purchases) kept behind a parental gate.

### 3.5 Library and offline

- One-tap save of a story to the device.
- **Saved items:** both the audio file and the story text, so a story can be replayed offline without regeneration.
- Saved stories play with no network connection.

## 4. Non-functional requirements

| Category | Requirement |
|----------|-------------|
| Latency | Audio begins within about 4 seconds of the final icon pick. Needs a prototype: stream LLM output sentence by sentence into TTS and mitigate serverless cold starts. |
| Offline | Saved stories play fully offline. |
| Battery | Under 3% drain per 30 minutes of screen-off playback, measured on a defined reference device. |
| Privacy | Compliant with COPPA, UK GDPR Article 8 and the ICO Children's Code (section 6). |
| Cost | Per-story LLM and TTS cost is tracked from day one, since it drives the paywall design. |

## 5. Technical overview

- **Frontend:** Flutter or React Native (iOS and Android).
- **Backend:** Python and FastAPI on a serverless platform.
- **LLM:** Gemini Flash or GPT-4o class model with a bedtime system prompt.
- **TTS:** ElevenLabs or Google Cloud TTS with streaming.
- **Data:** Supabase or Firebase for metadata; local storage for offline audio.
- **Vendor requirement:** LLM and TTS providers must contractually not retain or train on child-related inputs.

## 6. Privacy and compliance

- **Voice prompting (mic button).** Removed from the MVP. It is the only feature that handles a child's voice, and recognition of 3-year-olds is unreliable anyway. Revisit later, with its own review.
- **Child's name.** A name sent to a third-party LLM is a child's personal data. Require verifiable parental consent, send only the first name, and avoid storing it server-side where possible.
- **Assessments.** Complete a data protection impact assessment, and sign data processing agreements with each vendor, before launch.
- **Analytics and ads.** No advertising and no third-party behavioural tracking in the child-facing experience.
- **App store rules.** If listed in a kids category, platform rules apply, including parental gates before purchases and limits on third-party analytics. Verify current Apple and Google requirements before submission.

## 7. Monetisation and paywall options

Stories cost real money to generate (LLM plus TTS), so the free tier must be capped. Options considered:

| Option | How it works | Pros | Cons |
|--------|--------------|------|------|
| **A. Freemium with story cap** | A few new stories free per week; unlimited and extras with a subscription | Parents try the product before paying; strong conversion at the natural moment (a child wants another story) | Free users still cost money; needs careful cap tuning |
| **B. Free trial, then subscription** | Full access for about 7 days, then paywall | Shows full value; simple to explain | Higher drop-off at the paywall; no free tier for word of mouth |
| **C. Hard paywall** | Subscription needed from first launch | Cleanest costs and revenue | Highest friction; parents may uninstall before seeing a story |
| **D. One-time purchase plus story packs** | Pay once, buy new quests, worlds or voices | No subscription fatigue | Does not match ongoing per-story generation cost |

**Recommendation: Option A, with a trial on the paid plan.**

- **Free:** about 3 new stories per week, 1 narrator, 2 soundscapes, and replays of saved stories.
- **Premium (monthly or annual):** unlimited new stories, all narrators and soundscapes, child name personalisation, unlimited offline saves and future quests.
- **Why:** it matches revenue to the main cost driver, and the paywall appears when a child asks for one more story, which is a natural upgrade moment.
- **Hold back for decisions:** price points, the exact free cap and the offline-save limit should be tested, not assumed.

**Paywall rules for a children's app**

- The paywall is only ever shown to a grown-up, behind a parental gate.
- Never show it mid-story or at bedtime wind-down. Show it at story selection.
- No countdown timers, guilt messaging or dark patterns aimed at children or tired parents.
- Make cancellation easy and the renewal terms clear.

## 8. Roadmap

| Phase | Focus | Deliverables |
|-------|-------|--------------|
| **1 (MVP)** | Core story loop | 3-icon picker with 6 quests, 3 narrators, 4 ambient loops, sleep timer, offline saving, opt-in child name, freemium paywall |
| **2** | Story library and variety | Story history, recurring characters across nights, more narrators and soundscapes, seasonal quests, optional voice prompting after review |
| **3** | Smart speakers | Alexa skill. Google Home path to be confirmed, as Google retired Conversational Actions in 2023, so check the current supported route |

## 9. Open questions

1. Which three narrator voices and four soundscapes ship in the MVP?
2. Final free-tier cap and subscription pricing?
3. Which reference device for battery testing?
4. Is voice prompting cut from MVP permanently or only deferred?
5. Which app-store category (kids or family) and its rule set?
6. Which LLM and TTS vendors meet the no-retention requirement?

## 10. Changes from v1.0

- Removed parent voice cloning, the habit tracker and behaviour-reinforcement themes.
- Replaced the Lesson theme with six gentle quests.
- Moved child name insertion into the MVP, consent-gated.
- Removed voice prompting from the MVP.
- Dropped unsourced statistics and health claims.
- Added the monetisation and paywall section.
- Flagged the Google Home integration path for verification.
