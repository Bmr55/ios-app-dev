# Pre-Xcode Preparation Guide for the Location Audio Game

## Purpose

This guide is a practical plan for doing as much useful work as possible before paying for cloud Mac time and doing the actual iOS development in Xcode.

The main goal is to make paid Mac time focused on Apple-specific work:

- Creating and configuring the Xcode project
- Building and signing the app
- Testing location permissions
- Testing background location behavior
- Testing text-to-speech audio
- Running on simulator and iPhone
- Uploading to TestFlight later

Most product design, game rules, data modeling, fake route testing, and server planning can happen before that.

## Project Summary

The app is a location-based audio game for iPhone. A player starts a timed session, moves through a real-world area, and scores by completing location-driven challenges. The player should not need to stare at the screen while playing. The app uses GPS updates to detect nearby objectives and speaks short prompts through headphones.

The broader concept includes:

- Speed traps
- Speed zones
- Landmarks
- Random point targets
- Unique street scoring
- Hazard reporting
- Foreground setup and sync
- Local gameplay while moving
- Audio-first user experience

The first implementation should be smaller than the full vision. The first iOS prototype should prove the core technical loop:

```text
Load game data
-> Start session
-> Process simulated or real location updates
-> Trigger game events
-> Speak audio feedback
-> Show a basic score summary
```

## Guiding Principle

Do not spend paid Xcode time deciding what the game is.

Use local prework to answer as many questions as possible before opening Xcode:

- What data does a game session need?
- What does each challenge type contain?
- How are points awarded?
- What events cause audio messages?
- How does a session start and end?
- How do hazard reports affect game placement?
- What should happen when permissions are denied?
- What can be mocked for the first prototype?

Xcode time should mostly be used to turn known behavior into an iOS app.

---

# Phase 1: Define the MVP

## Why This Matters

The full idea is large. If the first Xcode build tries to include everything, too much time will be spent sorting out product scope while also learning iOS implementation details.

The MVP should prove the smallest meaningful version of the app.

## Recommended MVP

The first playable build should include:

- A basic home screen
- A mock or bundled game definition
- A manual `Start Game` button
- A session timer
- Simulated GPS playback or real location updates
- Detection of nearby triggers
- Text-to-speech prompts
- Basic scoring
- A final session summary

The first build should not require:

- A real backend
- User accounts
- Multiplayer
- App Store polish
- Full map editing
- Complete hazard workflows
- Advanced route generation
- Live server communication during gameplay

## MVP Feature Boundaries

### Include

- One sample play area
- One or two speed traps
- One speed zone
- Two or three landmarks
- A few random target events
- Basic unique street placeholder logic, if easy
- Basic score tracking
- Short audio prompts

### Defer

- Full hazard reporting
- Social features
- Leaderboards
- Complex map rendering
- Server-side game generation
- Route optimization
- Apple Watch support
- Full offline persistence

## MVP Success Criteria

The MVP succeeds if:

- The app can load a game definition.
- The user can start a session.
- Location updates, real or simulated, cause game events.
- Game events produce spoken audio.
- The score changes correctly.
- The session ends cleanly.
- Location updates stop after the game ends.

---

# Phase 2: Create the Game Data Model

## Why This Matters

The data model is one of the best things to design before Xcode. If the JSON structure is clear, the Swift `Codable` models become straightforward.

The game should be driven by data wherever possible. The iOS app should not have challenge locations hardcoded into view code.

## Core Data Files to Design

Create local JSON examples for:

- `GameDefinition`
- `GameSession`
- `GameTrigger`
- `SpeedTrap`
- `SpeedZone`
- `Landmark`
- `RandomTarget`
- `UniqueStreetRule`
- `HazardReport`
- `PlayerProgress`
- `AudioEvent`

## Example Game Definition

```json
{
  "gameId": "test-area-001",
  "title": "Neighborhood Sprint",
  "durationSeconds": 1200,
  "movementMode": "cycling",
  "playArea": {
    "type": "circle",
    "center": {
      "latitude": 40.4406,
      "longitude": -79.9959
    },
    "radiusMeters": 1200
  },
  "challenges": [
    {
      "id": "trap-bridge-01",
      "type": "speedTrap",
      "title": "Bridge Trap",
      "latitude": 40.4406,
      "longitude": -79.9959,
      "radiusMeters": 35,
      "onceOnly": true,
      "audioPrompt": "Speed trap ahead near the bridge."
    },
    {
      "id": "landmark-park-01",
      "type": "landmark",
      "title": "Park Entrance",
      "latitude": 40.4411,
      "longitude": -79.9948,
      "radiusMeters": 40,
      "points": 50,
      "audioPrompt": "Landmark nearby. Park entrance."
    }
  ]
}
```

## Recommended Fields for Every Challenge

Each challenge should probably include:

- `id`
- `type`
- `title`
- `location` or geometry
- `radiusMeters`
- `points` or scoring parameters
- `audioPrompt`
- `onceOnly`
- `movementModes`
- `disabledByHazards`

## Geometry Types to Support

Start simple. Support:

- Point with radius
- Segment with start and end points
- Area later

Suggested geometry model:

```json
{
  "geometry": {
    "type": "point",
    "latitude": 40.4406,
    "longitude": -79.9959,
    "radiusMeters": 35
  }
}
```

Later:

```json
{
  "geometry": {
    "type": "segment",
    "start": {
      "latitude": 40.4401,
      "longitude": -79.9962
    },
    "end": {
      "latitude": 40.4410,
      "longitude": -79.9949
    },
    "corridorMeters": 30
  }
}
```

## Pre-Xcode Deliverables

Before renting cloud Mac time, create:

- One valid sample game JSON file
- One sample progress JSON file
- One sample hazard report JSON file
- Notes describing each field
- A list of required versus optional fields

---

# Phase 3: Build the Game Engine Outside Xcode

## Why This Matters

The game engine is mostly logic. It does not need SwiftUI, Xcode, signing, or an iPhone to be designed.

The engine can be prototyped locally in TypeScript, JavaScript, Python, or pseudocode. The purpose is to validate rules before dealing with Apple APIs.

## What the Engine Should Do

Given:

- A game definition
- A current session state
- A stream of location updates
- Current time

The engine should return:

- Newly triggered events
- Score changes
- Audio messages to speak
- Updated session state

## Core Engine Questions

Answer these before Xcode:

- How close must the player be to trigger a speed trap?
- Can a speed trap fire more than once?
- How is speed measured for a trap?
- How does the player enter a speed zone?
- How does the player complete a speed zone?
- What happens if the player leaves a zone halfway through?
- How are landmarks scored?
- How often do random targets appear?
- How long do random targets last?
- How are unique streets detected?
- What audio message has priority when multiple things happen?
- What happens when the session timer expires?

## Suggested Engine Interface

```text
GameEngine.processLocation(update, sessionState, gameDefinition)
  -> updatedSessionState
  -> scoreEvents
  -> audioEvents
```

Example input:

```json
{
  "timestampSeconds": 15,
  "latitude": 40.4406,
  "longitude": -79.9959,
  "speedMetersPerSecond": 6.8,
  "horizontalAccuracyMeters": 8
}
```

Example output:

```json
{
  "scoreEvents": [
    {
      "type": "speedTrapTriggered",
      "challengeId": "trap-bridge-01",
      "points": 120
    }
  ],
  "audioEvents": [
    {
      "priority": "high",
      "message": "Trap hit. Six point eight meters per second. One hundred twenty points."
    }
  ]
}
```

## Pre-Xcode Deliverables

Before Xcode, create:

- A plain-language rules document
- A small executable prototype, if possible
- Several fake location traces
- Expected score results for each trace
- A list of edge cases

---

# Phase 4: Create Fake GPS Traces

## Why This Matters

Testing a location game by physically moving around is slow. Fake GPS traces let you test the game loop repeatedly and quickly.

These traces can later be used in the iOS app as debug inputs.

## Trace Format

Use a simple JSON array:

```json
[
  {
    "timestampSeconds": 0,
    "latitude": 40.4401,
    "longitude": -79.9962,
    "speedMetersPerSecond": 2.5,
    "horizontalAccuracyMeters": 10
  },
  {
    "timestampSeconds": 5,
    "latitude": 40.4403,
    "longitude": -79.9960,
    "speedMetersPerSecond": 4.1,
    "horizontalAccuracyMeters": 9
  },
  {
    "timestampSeconds": 10,
    "latitude": 40.4406,
    "longitude": -79.9959,
    "speedMetersPerSecond": 6.8,
    "horizontalAccuracyMeters": 8
  }
]
```

## Recommended Test Traces

Create traces for:

- Player misses all objectives
- Player hits one speed trap
- Player hits the same speed trap twice
- Player enters and completes a speed zone
- Player enters a speed zone but exits incorrectly
- Player collects a landmark
- Player collects a random target before it expires
- Player reaches a random target after it expires
- Player has poor GPS accuracy
- Player pauses or stops moving
- Session timer expires mid-route

## Expected Result Files

For each trace, write what should happen:

```text
Trace: speed-trap-basic.json

Expected:
- At t=10, trigger trap-bridge-01.
- Award 120 points.
- Queue "Trap hit..." audio message.
- Do not trigger trap-bridge-01 again later.
```

This lets you test the engine without guessing.

---

# Phase 5: Design the Audio Experience

## Why This Matters

This app is audio-first. The audio prompts are not decorative; they are the main interface during play.

Short, clear prompts should be written before implementation.

## Audio Design Rules

Prompts should be:

- Short
- Calm
- Unambiguous
- Useful without looking at the screen
- Safe for walking, running, or cycling
- Not overly frequent

Avoid prompts that require the player to look at the phone while moving.

## Prompt Categories

Create prompt text for:

- Session start
- Time remaining
- Nearby speed trap
- Speed trap triggered
- Nearby speed zone
- Speed zone started
- Speed zone completed
- Landmark nearby
- Landmark reached
- Random target appeared
- Random target collected
- Random target expired
- Unique street milestone
- Hazard warning
- Session ending soon
- Game over

## Example Prompt Library

```text
Session start:
"Game started. Twenty minutes on the clock."

Speed trap nearby:
"Speed trap ahead."

Speed trap hit:
"Trap hit. One hundred twenty points."

Speed zone started:
"Zone started."

Speed zone completed:
"Zone complete. Strong pace. Two hundred points."

Landmark reached:
"Landmark reached. Fifty points."

Random target appeared:
"New target nearby."

Game over:
"Game over. Final score: four hundred twenty."
```

## Audio Priority

Define priority before implementation:

1. Safety or hazard warning
2. Game state change
3. Challenge started or completed
4. Score update
5. Optional nearby opportunity
6. Ambient encouragement

If several events happen at once, the app should not speak all of them immediately. It should pick the most important message or combine them.

## Pre-Xcode Deliverables

Before Xcode, create:

- A prompt library document
- A priority list
- Rules for suppressing repeated prompts
- Rules for combining messages

---

# Phase 6: Define the App State Machine

## Why This Matters

Location and background audio apps need a clear lifecycle. A state machine prevents confusing behavior like GPS running after the game ends.

## Recommended States

```text
Idle
Preparing
PermissionNeeded
Ready
Running
Paused
Syncing
Ended
Error
```

## State Responsibilities

### Idle

- No active game
- Location updates are off
- Speech is inactive
- User can select or load a game

### Preparing

- Game data is being loaded
- Permissions may be checked
- Audio may be configured
- User cannot start until setup succeeds

### PermissionNeeded

- The app needs location or audio-related permission
- User sees a clear explanation
- Gameplay cannot start yet

### Ready

- Game data is loaded
- Permissions are sufficient
- User can tap `Start Game`
- Timer has not started

### Running

- Timer is active
- Location updates are active
- Game engine processes movement
- Speech messages may play
- Score changes are recorded

### Paused

- Timer may pause, depending on design
- Location processing may stop or reduce
- Speech should be quiet except for confirmation

### Syncing

- App is in foreground
- Local progress is being uploaded
- Server may send updated state

### Ended

- Timer has stopped
- Location updates are stopped
- Speech queue is cleared or finalized
- Final summary is visible

### Error

- Something failed
- App should show a recoverable message
- Location should not continue unless explicitly needed

## Transition Examples

```text
Idle -> Preparing
Preparing -> PermissionNeeded
PermissionNeeded -> Ready
Ready -> Running
Running -> Paused
Paused -> Running
Running -> Syncing
Syncing -> Running
Running -> Ended
Any State -> Error
Error -> Idle
```

## Pre-Xcode Deliverables

Before Xcode, create:

- A state machine diagram or table
- A list of allowed transitions
- Notes about what starts and stops in each state
- Permission failure behavior
- Session end behavior

---

# Phase 7: Plan the Screens

## Why This Matters

The first iOS app does not need beautiful UI, but it does need clear screens. Simple wireframes reduce time spent improvising in SwiftUI.

## Recommended MVP Screens

### Home Screen

Purpose:

- Load or select a sample game
- Show basic game title and duration
- Move to setup

### Setup Screen

Purpose:

- Show movement mode
- Show duration
- Show permission status
- Let user start the game

### Active Game Screen

Purpose:

- Show timer
- Show score
- Show current state
- Show most recent audio event
- Provide an obvious end button

### Session Summary Screen

Purpose:

- Show final score
- Show challenge breakdown
- Show triggered events
- Allow starting over

### Hazard Reporting Prototype Screen

This can be deferred. If included, keep it simple:

- Mark current location
- Select hazard category
- Save local hazard report

## Wireframe Deliverables

Before Xcode, create rough wireframes for:

- Home
- Setup
- Active game
- Summary
- Hazard reporting, if included

These can be plain text, drawings, screenshots from another tool, or simple Markdown.

---

# Phase 8: Plan Hazard Reporting Separately

## Why This Matters

Hazard reporting is important, but it can easily become a whole second product. It should be designed as a separate mode, not mixed into timed gameplay.

## Hazard Reporting MVP

Start with:

- Point hazard only
- Current location only
- Category picker
- Optional note
- Movement mode affected
- Local save

Defer:

- Segment drawing
- Area drawing
- Editing previous reports
- Server moderation
- User trust scoring
- Public hazard maps
- Advanced hazard expiration rules

## Hazard Categories

Use the categories from the design:

- Construction, passable
- Road closed
- Bad surface
- Blocked bike lane or shoulder
- Sidewalk or path blocked
- Debris or obstruction
- Water, ice, snow, or mud
- Poor lighting or visibility
- Heavy traffic or unsafe crossing
- Crowds or events

## Hazard Report Data Model

```json
{
  "id": "hazard-001",
  "createdAt": "2026-05-10T15:30:00Z",
  "category": "badSurface",
  "geometry": {
    "type": "point",
    "latitude": 40.4406,
    "longitude": -79.9959,
    "radiusMeters": 25
  },
  "affectedModes": ["cycling"],
  "severity": "medium",
  "note": "Rough pavement and loose gravel near the corner.",
  "status": "active"
}
```

## How Hazards Affect the Game

Define rules such as:

- Road closed: do not place objectives there.
- Bad surface: avoid speed traps and speed zones.
- Blocked bike lane: avoid cycling speed objectives.
- Sidewalk blocked: avoid walking and running objectives.
- Poor lighting: avoid night objectives.
- Crowds or events: reduce objective value or warn the player.

## Pre-Xcode Deliverables

Before Xcode, create:

- Hazard report JSON examples
- A category list
- A severity list
- Rules for how hazards suppress game objectives
- A decision on whether hazard reporting is in MVP 1

---

# Phase 9: Plan the Backend Without Building It First

## Why This Matters

The first iOS prototype should not depend on a full backend. However, planning the API early prevents the local data model from becoming incompatible with future server work.

## Recommended First Approach

Use local bundled JSON or a static JSON file for the first Xcode prototype.

Later, replace it with real networking.

## Future API Sketch

```text
GET /games/:id
POST /sessions/start
POST /sessions/:id/progress
POST /sessions/:id/end
POST /hazards
GET /hazards?bbox=...
```

## Example Start Session Request

```json
{
  "gameId": "test-area-001",
  "movementMode": "cycling",
  "startedAt": "2026-05-10T15:30:00Z"
}
```

## Example Progress Update

```json
{
  "sessionId": "session-001",
  "elapsedSeconds": 620,
  "score": 420,
  "triggeredChallengeIds": [
    "trap-bridge-01",
    "landmark-park-01"
  ]
}
```

## Pre-Xcode Deliverables

Before Xcode, create:

- A draft API contract
- Example request JSON
- Example response JSON
- A decision to use local JSON for MVP 1

---

# Phase 10: Prepare iOS-Specific Decisions

## Why This Matters

Some choices should be made before renting a Mac. They are not difficult, but they can waste time if decided during setup.

## Project Settings to Decide

- App name
- Bundle identifier
- Organization identifier
- Minimum iOS version
- SwiftUI or UIKit
- Portrait only or multiple orientations
- Whether iPad is supported
- Whether Apple Developer Program enrollment is ready

## Recommended Choices for MVP

- Interface: SwiftUI
- Language: Swift
- Minimum iOS version: a recent version that supports modern SwiftUI comfortably
- Device target: iPhone first
- Orientation: portrait first
- Data source: bundled JSON
- Backend: mocked or deferred

## Required Apple Capabilities

The app will likely need:

```text
Background Modes
- Location updates
- Audio, AirPlay, and Picture in Picture
```

Actual needs should be confirmed during development, but these are the likely capabilities for an active background location audio session.

## Permission Text to Draft

Draft these before Xcode:

```text
NSLocationWhenInUseUsageDescription
This game uses your location during active gameplay to trigger nearby audio events.
```

```text
NSLocationAlwaysAndWhenInUseUsageDescription
This game uses location during active sessions so it can continue triggering audio events when your phone is locked or in your pocket.
```

The wording should be honest, calm, and specific.

## Pre-Xcode Deliverables

Before Xcode, prepare:

- App name
- Bundle identifier
- Permission text
- Capability checklist
- Apple account status
- Device testing plan

---

# Phase 11: Prepare the Cloud Mac Session

## Why This Matters

Cloud Mac time should be treated like a focused work session. The more setup you have ready, the less time you pay for configuration and decision-making.

## Before Renting

Have ready:

- Git repository
- MVP definition
- Sample game JSON
- Fake GPS traces
- Audio prompt library
- State machine document
- Screen wireframes
- App name and bundle ID
- Apple account credentials
- Apple Developer Program status, if needed
- iPhone for device testing, if available

## First Cloud Mac Session Goal

The first session should produce a clean starter app that builds.

Recommended tasks:

1. Create the Xcode project.
2. Configure bundle identifier.
3. Configure signing.
4. Commit the clean starter project.
5. Add basic SwiftUI screens.
6. Add bundled sample JSON.
7. Decode JSON into Swift models.
8. Add a fake location playback service.
9. Add a simple game engine shell.
10. Add speech synthesis.
11. Run in simulator.
12. Push all changes to Git.

## First Cloud Mac Session Non-Goals

Avoid spending the first session on:

- Full backend integration
- Full hazard reporting
- Complex map UI
- App Store screenshots
- Perfect visual design
- Advanced scoring balance
- Leaderboards

## Second Cloud Mac Session Goal

The second session can focus on real iOS behavior:

- CoreLocation permissions
- Real location updates
- Background location behavior
- Audio behavior while locked
- Session end cleanup
- Testing on iPhone

## Third Cloud Mac Session Goal

The third session can focus on polish and architecture:

- Cleaner state management
- Better UI
- Error states
- Local persistence
- More test data
- Basic progress sync, if needed

---

# Phase 12: Suggested Local File Structure

Before creating the Xcode project, organize prework like this:

```text
location-audio-game-prework/
  docs/
    mvp.md
    rules.md
    state-machine.md
    audio-prompts.md
    api-contract.md
    hazard-rules.md
  data/
    sample-game.json
    sample-progress.json
    sample-hazards.json
  traces/
    speed-trap-basic.json
    speed-zone-complete.json
    landmark-basic.json
    random-target-expired.json
  expected-results/
    speed-trap-basic.md
    speed-zone-complete.md
    landmark-basic.md
    random-target-expired.md
  prototype/
    README.md
```

If you build a local engine prototype, place it in `prototype/`.

---

# Phase 13: Pre-Xcode Checklist

Use this checklist before renting cloud Mac time.

## Product Scope

- [ ] MVP is clearly defined.
- [ ] Deferred features are listed.
- [ ] First test area is chosen.
- [ ] First movement mode is chosen.
- [ ] Session duration options are chosen.

## Game Data

- [ ] Sample game JSON exists.
- [ ] Challenge types are defined.
- [ ] Required fields are documented.
- [ ] Sample progress JSON exists.
- [ ] Sample hazard JSON exists.

## Game Rules

- [ ] Speed trap rules are defined.
- [ ] Speed zone rules are defined.
- [ ] Landmark rules are defined.
- [ ] Random target rules are defined.
- [ ] Unique street rules are defined or deferred.
- [ ] Scoring rules are defined.
- [ ] Timer behavior is defined.

## Location Testing

- [ ] Fake GPS trace format is defined.
- [ ] At least five trace files exist.
- [ ] Expected results are written for each trace.
- [ ] Poor GPS accuracy behavior is considered.

## Audio

- [ ] Prompt library exists.
- [ ] Audio priority rules are defined.
- [ ] Repeated prompt suppression is defined.
- [ ] Game-over audio is defined.

## App Lifecycle

- [ ] State machine is defined.
- [ ] Allowed transitions are listed.
- [ ] Start behavior is defined.
- [ ] Pause behavior is defined.
- [ ] End behavior is defined.
- [ ] Sync behavior is defined or deferred.

## Hazard Reporting

- [ ] Hazard reporting is included or deferred for MVP 1.
- [ ] Hazard categories are defined.
- [ ] Hazard JSON format is drafted.
- [ ] Hazard impact rules are drafted.

## iOS Setup

- [ ] App name is chosen.
- [ ] Bundle identifier is chosen.
- [ ] Apple account is ready.
- [ ] Developer Program status is known.
- [ ] Permission text is drafted.
- [ ] Background capabilities are listed.
- [ ] Test device plan is ready.

## Cloud Mac Readiness

- [ ] Git repo is ready.
- [ ] Files are backed up.
- [ ] First cloud Mac session tasks are listed.
- [ ] Non-goals are listed.
- [ ] Login credentials are available.

---

# Recommended Order of Work

## Do First

1. Write the MVP definition.
2. Create sample game JSON.
3. Create fake GPS traces.
4. Define scoring rules.
5. Write audio prompts.
6. Define the state machine.

## Do Second

1. Prototype game logic locally.
2. Test fake traces against the rules.
3. Draft API contracts.
4. Draft hazard report data.
5. Create wireframes.

## Do Right Before Renting the Mac

1. Prepare app name and bundle ID.
2. Prepare permission text.
3. Prepare Apple account details.
4. Put all prework in Git.
5. Write the exact first-session task list.

## Do During First Xcode Session

1. Create project.
2. Configure signing.
3. Commit starter app.
4. Add models.
5. Decode sample JSON.
6. Add fake location playback.
7. Add speech output.
8. Build and run.

---

# Practical Advice

The biggest savings will come from separating three kinds of work:

## Product Work

This includes rules, screens, audio copy, scoring, challenge types, and hazard categories.

Do this before Xcode.

## Logic Work

This includes trigger detection, scoring, fake routes, timers, and event queues.

Do as much as possible before Xcode.

## Apple Integration Work

This includes CoreLocation, AVFoundation, background modes, signing, simulator testing, and device testing.

Do this in Xcode.

That separation keeps cloud Mac time focused and makes the first iOS build much less chaotic.

---

# Final Target Before Paying for Cloud Mac Time

Before renting cloud Mac space, aim to have:

- A clear MVP
- Sample game JSON
- Fake location traces
- Expected gameplay results
- Audio prompt library
- State machine
- Basic wireframes
- Hazard report model
- Draft API contract
- iOS setup checklist
- First Xcode session checklist

If those are ready, the first cloud Mac session can be used to build instead of decide.
