# Signal Run GPS Test User Guide

## What This App Is For

Signal Run GPS Test is a field-testing app for checking whether an iPhone can accurately detect game scoring events while you move outdoors.

You will use it to test:

- Landmarks
- Speed traps
- Speed zones
- Unique street or path segments
- GPS accuracy
- Spoken scoring feedback
- CSV export after a test session

This is not the finished Signal Run game. It is a testing tool.

## Before You Start

Use a safe outdoor area with clear sky view and low traffic.

Recommended places:

- Park paths
- Campus paths
- Trail loops
- Empty parking lots
- Quiet neighborhood streets
- Wide bike paths

Avoid:

- Busy roads
- Intersections
- Blind corners
- Crowded paths
- Areas with poor visibility
- Places where you need to look at the phone while moving

If you are cycling, set up all test points before riding.

## First Launch Setup

1. Open the app.
2. Go to the **Setup** tab.
3. Tap **Request Location Permission**.
4. Allow location access.
5. Tap **Refresh Current Location**.
6. Confirm that the app shows a GPS accuracy value.

The app works best when the accuracy is near or below the configured limit. The default minimum usable accuracy is `20 m`.

If the accuracy is worse than expected, wait outside for a better GPS fix before placing objectives.

## Understanding The Tabs

### Setup

Use this tab to:

- Request location permission
- Check current GPS accuracy
- Choose movement mode
- Set minimum usable GPS accuracy
- Start a test session
- End a test session

### Objectives

Use this tab to create test scoring points.

Objectives are placed using the iPhone's current GPS location.

### Live

Use this tab during a test session to see:

- Current score
- Number of GPS samples
- Number of scoring events
- Recent scoring decisions
- Reasons why objectives did or did not score

### Results

Use this tab after a session to:

- Review the final score
- See scored events
- Prepare a CSV export
- Share the CSV file

## Placing A Landmark

A landmark is a point that scores when you enter its radius.

Use this for:

- Public art
- Trailheads
- Bridges
- Park entrances
- Campus buildings
- Visible route markers

Steps:

1. Go to the real-world landmark.
2. Open the **Objectives** tab.
3. Tap **Refresh Current Location**.
4. Choose **Landmark**.
5. Enter a name.
6. Set the radius.
7. Use `10 m` for a strict test.
8. Use `15 m` or `25 m` for easier early testing.
9. Tap **Add at Current Location**.

During the session, the landmark should score when you enter the radius.

## Placing A Speed Trap

A speed trap is a point that scores when you pass through it. It can also award speed bonuses.

Use this only on a safe straight path.

Good speed trap locations:

- Straight bike path
- Clear park road
- Open trail segment
- Quiet low-traffic street

Avoid speed traps near:

- Intersections
- Stop signs
- Tight turns
- Crosswalks
- Crowds
- Poor surfaces

Steps:

1. Go to the exact point where the trap should trigger.
2. Open the **Objectives** tab.
3. Tap **Refresh Current Location**.
4. Choose **Speed Trap**.
5. Enter a name.
6. Set the radius.
7. Set speed thresholds if needed.
8. Tap **Add at Current Location**.

Default cycling thresholds:

```text
Strong: 10 mph
Excellent: 15 mph
```

During the session, ride through the trap location. The app uses recent valid GPS speed near the trap to decide the score tier.

## Placing A Speed Zone

A speed zone has a start gate and a finish gate.

Use this for testing point-to-point scoring.

Choose a safe segment where you can ride from start to finish without stopping or making risky turns.

Steps:

1. Go to the start of the segment.
2. Open the **Objectives** tab.
3. Tap **Refresh Current Location**.
4. Choose **Speed Zone**.
5. Enter a name.
6. Set the gate width.
7. Tap **Use Current Location as Start**.
8. Move to the finish point.
9. Tap **Refresh Current Location**.
10. Tap **Use Current Location as Finish**.
11. Tap **Add Segment Objective**.

During the session, you must enter the start gate first and then the finish gate.

Default cycling thresholds:

```text
Strong average: 9 mph
Excellent average: 13 mph
```

If you enter the finish before the start, the app logs `wrongGate`.

## Placing A Unique Street Or Path Segment

A unique street objective tests whether the app can detect that you traveled along a manually defined street, trail, or path segment.

This is an early test version of future named-street scoring.

Steps:

1. Go to one end of the street or path segment.
2. Open the **Objectives** tab.
3. Tap **Refresh Current Location**.
4. Choose **Unique Street**.
5. Enter a name.
6. Set the corridor width.
7. Tap **Use Current Location as Start**.
8. Move to the other end of the segment.
9. Tap **Refresh Current Location**.
10. Tap **Use Current Location as Finish**.
11. Tap **Add Segment Objective**.

During the session, ride along the segment. The app scores it when enough GPS samples are inside the corridor and you travel enough of the segment.

## Running A Test Session

1. Create the objectives you want to test.
2. Go to **Setup**.
3. Confirm the movement mode.
4. Confirm the minimum usable accuracy.
5. Tap **Start Test Session**.
6. Put the phone where you want to test it:
   - Mounted on bike
   - In pocket
   - In bag
   - Handheld while walking
7. Move through the test area.
8. Listen for spoken scoring messages.
9. When finished, stop moving safely.
10. Tap **End Test Session**.

Do not interact with the phone while cycling.

## Reading Live Results

The **Live** tab shows recent scoring decisions.

Useful result values:

| Result | Meaning |
|---|---|
| `fired` | A landmark, trap, or street scored |
| `started` | A zone or segment began |
| `active` | A zone or segment is in progress |
| `completed` | A speed zone completed |
| `rejected` | The sample did not score |

Useful rejection reasons:

| Reason | Meaning |
|---|---|
| `poorAccuracy` | GPS accuracy was worse than the configured limit |
| `outsideRadius` | You were not inside the objective radius |
| `outsideGates` | You were not inside either speed-zone gate |
| `wrongGate` | You reached the finish before the start |
| `outsideCorridor` | You were too far from the street/path segment |
| `alreadyCompleted` | The objective already scored this session |

These reasons help explain why something did or did not score.

## Exporting Test Data

After ending a session:

1. Go to **Results**.
2. Tap **Prepare CSV Export**.
3. Tap **Share CSV**.
4. Send or save the file.

The CSV includes:

- Session details
- Objective definitions
- GPS samples
- Scoring events
- Accuracy values
- Speed values
- Distances to objectives
- Score changes
- Rejection reasons

This file is the main evidence for debugging GPS accuracy and scoring behavior.

## What To Report After Testing

When sending feedback, include:

- iPhone model
- iOS version
- Movement mode
- Whether the phone was mounted, pocketed, or handheld
- Test location type
- Whether headphones or speakers were used
- Whether the screen was locked
- What seemed accurate
- What seemed late, missed, or wrong
- The exported CSV file

Helpful notes:

- "Landmark scored about 5 seconds late."
- "Speed trap scored correctly on first pass but not second pass."
- "Speed zone started but never completed."
- "Accuracy was around 35 m near trees."
- "Worked while screen was locked."

## Safety Rules

- Do not use the screen while cycling.
- Place objectives before riding.
- Test in low-risk areas first.
- Start with walking tests if the route is unfamiliar.
- Keep volume low enough to hear your surroundings.
- Do not create speed objectives where speed would be unsafe.
- End the session when testing is finished.

## Quick Test Checklist

Before moving:

- Location permission granted
- Current location refreshed
- GPS accuracy looks acceptable
- Objectives created
- Movement mode selected
- Session started

After moving:

- Session ended
- Results reviewed
- CSV exported
- Notes written while the test is fresh

## Best First Test

For the first real field test, keep it simple:

1. Create one landmark with a `10 m` radius.
2. Create one landmark with a `20 m` radius nearby.
3. Start a session.
4. Walk or ride past both points.
5. End the session.
6. Export the CSV.

This test quickly shows whether `10 m` is realistic or whether the game needs larger default landmark radii.
