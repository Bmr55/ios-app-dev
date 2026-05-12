# Signal Run GPS Scoring Test Harness

## Purpose

The Signal Run GPS Scoring Test Harness is a basic native iOS application for testing whether iPhone GPS is accurate and reliable enough for the location-based scoring model described in the Signal Run game design.

This is not the polished game. It is a field-test tool.

The app is built to answer practical questions:

- Can an iPhone detect when a cyclist reaches a landmark-sized target?
- What trigger radius is reliable enough outdoors?
- Does GPS accuracy explain missed or late scoring events?
- Can speed traps use recent GPS speed without being too noisy?
- Can start/finish zones work for speed-zone scoring?
- Can manually defined street or path segments be detected well enough for unique-street scoring?
- Can testers export enough evidence to diagnose what happened after a ride?

The first version is intentionally local-only. It does not use accounts, servers, multiplayer, leaderboards, or live game downloads.

## Project Location

The app lives in:

```text
SignalRunGPSTestHarness.xcodeproj
SignalRunGPSTestHarness/
```

Main implementation files:

```text
SignalRunGPSTestHarnessApp.swift
Models.swift
ScoringEngine.swift
LocationService.swift
SpeechService.swift
TestHarnessStore.swift
Views.swift
CSVExporter.swift
Info.plist
```

## Technology

The app uses native Apple frameworks only:

| Framework | Purpose |
|---|---|
| SwiftUI | App screens, forms, navigation, and controls |
| CoreLocation | GPS permission, current location, live location updates, distance calculations |
| MapKit | Objective placement map and user location display |
| AVFoundation | Spoken audio feedback during test sessions |
| Foundation | Codable models, dates, file writing, CSV generation |
| ShareLink | Exporting CSV files through the iOS share sheet |

No third-party Swift packages are required.

## App Structure

The app has four main tabs.

### Setup

The Setup screen prepares the phone for a field test.

It shows:

- Current location permission state
- Current GPS accuracy when available
- Current GPS speed in mph when available
- Movement mode
- Minimum usable GPS accuracy
- Objective count
- Start and End session controls

The movement mode currently supports:

- Cycling
- Walking

Cycling is the intended first field-test mode. Walking is still useful for safer low-speed debugging.

The minimum usable accuracy defaults to `20 m`. If a location sample reports worse accuracy than this, the scoring engine rejects the sample and logs `poorAccuracy` for each objective. This is important because the test harness is meant to identify when GPS quality, not scoring logic, caused an event to fail.

### Objectives

The Objective Editor lets testers create objectives outdoors without editing code.

The tester can refresh the current GPS location and create objectives from the phone's current position.

Supported objective types:

- Landmark
- Speed Trap
- Speed Zone
- Unique Street

The map displays the tester's current location and saved objective markers.

### Live

The Live Test screen shows what is happening during an active session.

It displays:

- Whether the session is running
- Current score
- Number of location samples recorded
- Number of scoring events recorded
- Latest GPS accuracy
- Latest GPS speed
- Recent scoring decisions

The recent decision list is deliberately diagnostic. It shows not only successful scoring events, but also rejection reasons such as:

- `poorAccuracy`
- `outsideRadius`
- `outsideGates`
- `wrongGate`
- `outsideCorridor`
- `alreadyCompleted`

This makes the app useful in the field. A tester should be able to tell whether something failed because the GPS was inaccurate, the trigger radius was too small, the objective was already completed, or the rider never entered the expected scoring area.

### Results

The Results screen summarizes the current or most recently completed session.

It shows:

- Final score
- Movement mode
- Number of objectives
- Number of GPS samples
- Number of scoring events
- Session duration
- Scored events

It also prepares and shares a CSV export. The export is the main evidence artifact for later analysis.

## Objective Types

### Landmark

A landmark is a point objective with a trigger radius.

Default behavior:

- The tester adds the landmark at the current GPS location.
- The default radius is `10 m`.
- The tester can adjust the radius.
- The landmark scores once per session.
- The default base score is `20`.

Scoring rule:

```text
If the user's current location is inside the landmark radius, score the landmark.
```

Common rejection reasons:

| Reason | Meaning |
|---|---|
| `poorAccuracy` | The GPS sample's horizontal accuracy was worse than the configured limit |
| `outsideRadius` | The user was not close enough to the landmark |
| `alreadyCompleted` | The landmark had already scored during this session |

Landmarks are the most important first GPS test because they answer the basic question: can the iPhone reliably detect arrival at a real-world point?

### Speed Trap

A speed trap is a point objective that scores when the rider passes through a radius around the trap.

Default behavior:

- The tester adds the trap at the current GPS location.
- The trap uses a configurable radius.
- The trap uses cycling speed thresholds by default.
- The trap scores once per session.
- The default base score is `10`.

Default cycling thresholds:

```text
Strong: 10 mph
Excellent: 15 mph
```

The scoring engine does not rely only on one location sample's speed. Instead, it keeps recent samples from the last few seconds and uses the best recent valid speed near the trap. This is meant to reduce noise from individual GPS speed readings.

Scoring tiers:

| Result | Score |
|---|---:|
| Trap hit | base score |
| Strong speed | base score + 10 |
| Excellent speed | base score + 20 |

Common rejection reasons:

| Reason | Meaning |
|---|---|
| `poorAccuracy` | GPS quality was too low |
| `outsideRadius` | The rider was not inside the trap radius |
| `alreadyCompleted` | The trap already scored this session |

### Speed Zone

A speed zone is a start-to-finish segment.

Default behavior:

- The tester captures a start point from the current GPS location.
- The tester captures a finish point from the current GPS location.
- The app treats each endpoint as a gate with configurable width.
- The zone starts when the rider enters the start gate.
- The zone completes when the rider later enters the finish gate.
- The default base score is `30`.

Default cycling thresholds:

```text
Strong average: 9 mph
Excellent average: 13 mph
```

Average speed is calculated from the straight-line distance between the start and finish coordinates divided by elapsed time between start-gate and finish-gate detection.

Scoring tiers:

| Result | Score |
|---|---:|
| Zone complete | base score |
| Strong average | base score + 15 |
| Excellent average | base score + 30 |

Common event reasons:

| Reason | Meaning |
|---|---|
| `zoneStarted` | The rider entered the start gate |
| `zoneActiveAwaitingFinish` | The rider has started the zone but has not reached the finish |
| `zoneCompleted` | The rider reached the finish gate |
| `zoneStrongAverage` | The completed zone met the strong speed threshold |
| `zoneExcellentAverage` | The completed zone met the excellent speed threshold |
| `wrongGate` | The rider reached the finish before entering the start |
| `outsideGates` | The rider is not close enough to either gate |
| `alreadyCompleted` | The zone already scored this session |

Speed zones are useful for testing whether GPS is good enough for ordered location detection, not just point arrival.

### Unique Street

A unique street objective is a manually defined segment with a corridor width.

Default behavior:

- The tester captures a segment start from the current GPS location.
- The tester captures a segment end from the current GPS location.
- The app creates a corridor around the segment.
- The objective scores when the rider spends enough samples inside the corridor and progresses far enough along the segment.
- The default base score is `5`.

Current scoring requirements:

```text
At least 3 samples inside the corridor
At least 55% progress along the segment
```

The implementation projects the user's location onto the manually defined segment and checks:

- distance from the segment
- fraction of progress along the segment
- number of samples inside the corridor

Common event reasons:

| Reason | Meaning |
|---|---|
| `streetSegmentStarted` | The rider entered the corridor |
| `insideCorridor` | The rider remains inside the corridor but has not completed enough progress |
| `uniqueStreetCompleted` | The rider traveled enough of the segment to score |
| `outsideCorridor` | The rider is too far from the segment |
| `alreadyCompleted` | The segment already scored this session |

This is a simple manual approximation of future named-street detection. It is useful for early tests before adding OpenStreetMap-style road data or map matching.

## Location Behavior

The app has two kinds of location usage.

### One-Shot Location Refresh

The tester can refresh the current location before a session to place objectives. This uses `requestLocation()` and does not start continuous tracking.

### Active Session Tracking

When the tester starts a session, the app starts continuous location updates.

Current location configuration:

```text
desiredAccuracy = kCLLocationAccuracyBest
distanceFilter = 5 m
activityType = fitness
pausesLocationUpdatesAutomatically = false
```

If Always permission is granted, the app enables background location updates and shows the background location indicator during active sessions.

When the tester ends the session:

- location updates stop
- background location updates are disabled
- the background location indicator is disabled
- speech is stopped and the final score is spoken

The app is designed so background location is tied to an active test session, not app launch.

## Audio Behavior

The app uses `AVSpeechSynthesizer` to speak simple session and scoring messages.

Audio behavior:

- Configures `AVAudioSession` with `.playback`
- Uses `.spokenAudio` mode
- Ducks other audio while speaking
- Queues speech so messages do not overlap

Examples:

```text
GPS scoring test started.
Landmark scored 20 points.
GPS scoring test ended. Final score 45.
```

The audio is deliberately minimal. The goal is to confirm that scoring events can be spoken during movement without requiring screen attention.

## Data Model

### TestObjective

Represents one scoring target.

Important fields:

| Field | Meaning |
|---|---|
| `id` | Stable UUID for this objective |
| `name` | Tester-visible objective name |
| `type` | Landmark, speed trap, speed zone, or unique street |
| `latitude`, `longitude` | Primary coordinate |
| `endLatitude`, `endLongitude` | Finish or segment end coordinate when needed |
| `radiusMeters` | Landmark or speed trap trigger radius |
| `gateWidthMeters` | Speed-zone gate width or unique-street corridor width |
| `speedThresholds` | Strong and excellent speed thresholds |
| `baseScore` | Base point value |
| `createdAt` | Objective creation time |

### LocationSample

Represents one GPS sample from CoreLocation.

Important fields:

| Field | Meaning |
|---|---|
| `timestamp` | When the location was recorded |
| `latitude`, `longitude` | GPS coordinate |
| `horizontalAccuracyMeters` | Apple's estimated horizontal accuracy |
| `speedMetersPerSecond` | GPS speed when available |
| `course` | Direction of travel when available |

Speed is stored in meters per second and displayed in mph.

### ScoringEvent

Represents a scoring decision.

Important fields:

| Field | Meaning |
|---|---|
| `timestamp` | Time of the decision |
| `objectiveId` | Related objective |
| `objectiveName` | Objective name |
| `objectiveType` | Objective type |
| `distanceMeters` | Distance from the relevant trigger, if available |
| `accuracyMeters` | GPS horizontal accuracy for the sample |
| `speedMetersPerSecond` | Sample speed or calculated speed |
| `scoreDelta` | Points awarded by this decision |
| `result` | Fired, rejected, started, active, or completed |
| `reason` | Human-readable diagnostic reason |

### TestSession

Represents one field test.

Important fields:

| Field | Meaning |
|---|---|
| `id` | Session UUID |
| `startedAt` | Start time |
| `endedAt` | End time |
| `movementMode` | Cycling or walking |
| `objectives` | Snapshot of objectives used for the session |
| `samples` | All recorded GPS samples |
| `events` | All scoring decisions |
| `finalScore` | Final score |

## CSV Export

The CSV export is the main analysis artifact.

The export contains three record types in one file:

- `session`
- `objective`
- `sample`
- `event`

Columns:

```text
record_type
session_id
timestamp
objective_id
objective_name
objective_type
latitude
longitude
end_latitude
end_longitude
radius_m
gate_width_m
accuracy_m
speed_mps
speed_mph
distance_m
score_delta
result
reason
final_score
```

The CSV is designed to answer questions like:

- What was the GPS accuracy when the rider passed the landmark?
- Was the rider outside the trigger radius, or did the sample get rejected for poor accuracy?
- Did the speed zone start but never finish?
- Did the rider enter the finish gate before the start gate?
- Was the speed trap hit, and what speed tier did it use?
- Did a unique street fail because the rider was outside the corridor or because there were not enough in-corridor samples?

## Recommended Field Test Flow

1. Open the app on an iPhone.
2. Go to Setup.
3. Request location permission.
4. Go to Objectives.
5. Refresh current location.
6. Create one or more objectives outdoors.
7. Go back to Setup and confirm the minimum usable accuracy.
8. Start a test session.
9. Put the phone in the realistic test position: mounted, pocketed, or locked.
10. Ride the test route.
11. End the session.
12. Go to Results.
13. Prepare and share the CSV export.

## Suggested First Tests

### Landmark Radius Test

Create the same landmark-style target with different radii:

```text
10 m
15 m
25 m
```

Ride past the location several times and compare:

- detection reliability
- distance at scoring time
- GPS accuracy at scoring time
- missed passes
- false positives

This test helps decide whether the game can use tight `10 m` landmark triggers or needs a larger default radius.

### Speed Trap Test

Place a speed trap on a safe straight section.

Ride through it at different speeds and compare:

- score tier
- best recent speed
- distance at trigger
- GPS accuracy

This test helps validate whether GPS speed is stable enough for performance scoring.

### Speed Zone Test

Mark a start gate and finish gate on a safe segment.

Test:

- normal start-to-finish ride
- entering near the finish first
- missing the finish gate
- slow vs fast completion

This test validates ordered detection and average speed scoring.

### Unique Street Segment Test

Mark a short street, path, or trail segment.

Test:

- riding along the full segment
- crossing the segment briefly
- riding parallel but outside the corridor
- repeating the same segment

This test helps validate whether simple corridor detection is enough before adding real map matching.

### Background Test

Start a session, lock the phone, and ride through one or more objectives.

Confirm:

- spoken events still happen
- events appear after reopening the app
- CSV includes the background samples and events
- location stops after ending the session

## Safety Notes

This app is designed for controlled field testing.

Recommended testing rules:

- Test in low-traffic areas.
- Do not interact with the screen while riding.
- Set up objectives before moving.
- Use headphones or audio routing safely.
- Keep volume low enough to hear surroundings.
- Start with walking tests before cycling if a route is unfamiliar.
- Avoid placing speed objectives near intersections, blind turns, crowds, poor surfaces, or traffic conflicts.

## Current Limitations

This version is intentionally small.

Known limitations:

- Objectives are stored only in memory during the app run.
- There is no server sync.
- There is no account system.
- There is no polished game loop or timer.
- Unique street detection uses manually drawn segments, not real street names.
- Speed zones use straight-line distance between start and finish.
- There are no automated tests yet.
- The project still needs to be built and tested on a Mac with Xcode and a real iPhone.

## What This Prototype Proves

This app should prove the foundation for Signal Run's real gameplay:

- GPS samples can drive local scoring.
- The app can distinguish successful triggers from rejected samples.
- Testers can create field objectives without code changes.
- Cycling speed thresholds can be tested in real-world conditions.
- Background location and speech can be validated on device.
- CSV exports can support practical post-ride analysis.

If this harness produces reliable landmark, speed trap, speed zone, and segment results, the next step is to turn the manually created objectives into loaded game definitions and move toward a playable Signal Run session.
