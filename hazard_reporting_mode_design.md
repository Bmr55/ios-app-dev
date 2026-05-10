# Hazard Reporting Mode Design

## High Concept

Hazard Reporting Mode is a non-game mode that lets users document real-world conditions that may make streets, paths, trails, or public spaces unpleasant, unsafe, inaccessible, or unsuitable for future game sessions.

The mode is designed for users who are already moving through an area, especially by bike, but it also supports reporting hazards later by selecting points, segments, or areas on a map.

The purpose is not to score points or compete. The purpose is to improve the shared quality of the play map so future games avoid bad locations, warn players about temporary issues, and place challenges in better spots.

## Design Goals

- Make hazard reporting fast, calm, and low-friction.
- Support both on-location reports and map-based reports.
- Distinguish between roads that are fully closed and roads that are technically usable but bad for gameplay.
- Capture whether a hazard affects walking, running, cycling, or all movement modes.
- Help future game sessions avoid unsafe or unpleasant routes.
- Keep the reporting mode clearly separate from timed gameplay.
- Avoid encouraging users to interact with the screen while actively moving.

## Relationship To The Game

Hazard reports should influence how game sessions are planned and evaluated.

A reported hazard might:

- Prevent a speed trap from appearing at that location.
- Prevent a speed zone from using a rough or blocked segment.
- Reduce the value of a landmark if the area is hard to access.
- Prevent random point targets from appearing inside a bad area.
- Make a street less attractive for unique street scoring.
- Add a warning before a player starts a session in the affected area.

Not every hazard means a place is unusable. Some hazards only mean the game should avoid placing fast or precision-based objectives there.

## Core Use Cases

### Ride-Based Reporting

The user is moving through the area and notices a hazard. They open Hazard Reporting Mode, mark the issue near their current location, and add a quick category.

This flow is for immediate observations:

- "This road is closed."
- "This street is rideable, but the surface is awful."
- "Construction makes this block a bad game segment."
- "The bike lane is blocked."

### Map-Based Reporting

The user selects a location on a map instead of physically moving to it. This supports reports made from memory, local knowledge, or after finishing a ride.

This flow is useful when:

- The user passed the hazard earlier but did not want to stop.
- The hazard is known but not directly accessible.
- The user wants to mark a whole road segment, not just a point.
- The user wants to correct or refine a previous report.

### Post-Ride Cleanup

The user reviews rough notes or quick markers after a ride and turns them into more complete hazard reports.

This supports safer reporting because the user can mark something quickly during movement, then fill in details later while stopped.

## Reporting Surfaces

Hazards can apply to different geographic shapes.

### Point Hazard

A specific spot, such as a pothole, fallen sign, blocked curb cut, or dangerous intersection.

### Segment Hazard

A stretch of street, path, trail, bridge, or lane. This is useful for construction zones, rough pavement, poor lighting, or long closures.

### Area Hazard

A broader zone, such as a plaza under construction, flooded park section, event crowd, or unsafe trail area.

The reporting experience should help the user choose the simplest shape that describes the problem.

## Hazard Categories

### Construction, Passable

The road, path, or area is still accessible, but the experience would be bad during a game.

Examples:

- Narrow temporary lanes
- Construction equipment near the route
- Gravel plates or uneven temporary surfaces
- Workers or cones creating confusion
- Loud, distracting, or unpleasant conditions

Game impact: avoid speed traps, avoid speed zones, avoid random targets, but the area may still count as passable for low-intensity exploration.

### Road Closed

The road, path, or segment is not usable for normal movement.

Examples:

- Full street closure
- Closed bridge
- Closed trail
- Blocked alley
- Police or event barricade

Game impact: treat as unavailable. Do not route through it or place objectives beyond it unless there is another clear access route.

### Bad Surface

The route is passable but unpleasant, slow, or risky because of surface quality.

Examples:

- Potholes
- Broken pavement
- Loose gravel
- Deep cracks
- Metal plates
- Mud
- Sand
- Root damage
- Brick or cobblestone too rough for speed play

Game impact: avoid speed challenges and consider reducing use for cycling sessions.

### Blocked Bike Lane Or Shoulder

The street may be open, but the part most useful for cycling is blocked.

Examples:

- Parked vehicles in the bike lane
- Construction signs in the shoulder
- Delivery zones occupying the lane
- Trash bins or debris in the riding line

Game impact: avoid cycling speed objectives and consider warning cyclists before play.

### Sidewalk Or Path Blocked

A walking or running route is partially or fully blocked.

Examples:

- Closed sidewalk
- Temporary fencing
- Outdoor dining overflow
- Utility work
- Tree work
- Snow piles

Game impact: avoid pedestrian-focused objectives and landmark approaches that depend on that access.

### Debris Or Obstruction

A physical object creates a hazard.

Examples:

- Fallen branches
- Glass
- Loose trash
- Construction debris
- Abandoned scooter or bike
- Downed sign

Game impact: avoid placing precision objectives nearby until cleared.

### Water, Ice, Snow, Or Mud

Weather or drainage makes the route unpleasant or unsafe.

Examples:

- Flooding
- Standing water
- Ice patches
- Packed snow
- Muddy trail
- Slippery leaves

Game impact: avoid affected segments while active, especially for speed zones.

### Poor Lighting Or Visibility

The location may be acceptable in daylight but poor at night or in low visibility.

Examples:

- Dark underpass
- Unlit trail
- Blind curve
- Hidden driveway
- Overgrown vegetation
- Confusing crossing

Game impact: avoid night sessions or speed objectives in the affected area.

### Heavy Traffic Or Unsafe Crossing

The location is technically open but stressful or risky for a game.

Examples:

- Fast vehicle traffic
- Difficult crossing
- Poor driver visibility
- Complicated intersection
- Frequent turning conflicts
- Narrow road with no shoulder

Game impact: avoid speed traps, speed zones, and random targets that would encourage rushed decisions.

### Crowds Or Events

The area is temporarily crowded or disrupted by an event.

Examples:

- Street fair
- Parade
- Market
- Stadium crowd
- School dismissal crowd
- Protest or public gathering

Game impact: temporarily avoid objectives that would send players through the area.

### Access Restricted

The location appears public on a map but is not actually appropriate for play.

Examples:

- Private road
- Gated campus area
- Service entrance
- Restricted park section
- Seasonal closure

Game impact: remove from normal play unless access is later confirmed.

### Other Hazard

For anything that does not fit the main categories.

The user should be able to add a short note so the report is still useful.

## Severity Levels

Each hazard should have a severity so the game can respond appropriately.

### Minor

The route is usable, but not ideal.

Game impact: avoid precision or speed objectives nearby.

### Moderate

The route is usable only with care or only for certain movement modes.

Game impact: avoid most objectives and warn users when relevant.

### Severe

The route should not be used for play.

Game impact: exclude from game placement and route planning.

### Closed

The route or area is unavailable.

Game impact: treat as blocked.

## Time Sensitivity

Hazards should be classified by how long they are expected to matter.

### Temporary

Likely to clear soon.

Examples:

- Debris
- Parked vehicle blocking a lane
- Event crowd
- Flooding after rain

### Multi-Day

Likely to last for several days or weeks.

Examples:

- Utility work
- Construction closure
- Trail repairs
- Seasonal path condition

### Long-Term

Likely to remain until infrastructure changes.

Examples:

- Consistently bad surface
- Dangerous crossing
- Poor lighting
- Permanently restricted access

### Unknown

The reporter is unsure.

This should be allowed because quick reports are still valuable.

## Affected Movement Modes

A hazard may affect one mode more than another.

The report should allow the user to mark:

- Walking
- Running
- Cycling
- All modes
- Not sure

Examples:

- Rough cobblestone may be bad for cycling but fine for walking.
- A blocked sidewalk may matter for walking and running but not cycling.
- A full road closure may affect every mode.
- Poor lighting may matter more for evening sessions.

## Report Details

A useful report should capture enough information to guide future gameplay without making reporting feel like paperwork.

Suggested fields:

- Hazard category
- Severity
- Location shape: point, segment, or area
- Affected movement modes
- Expected duration
- Short optional note
- Optional photo
- Whether the reporter is currently at the location or marking it from the map

The shortest valid report should require only category, severity, and location.

## Optional Photo Submission

Photo submission should be optional. A report should never require a photo, because many hazards can be reported safely and usefully without one.

Photos are most useful when the hazard is visual, ambiguous, or likely to be disputed.

Good photo candidates:

- Road closure signs
- Construction barriers
- Bad pavement
- Flooding or ice
- Blocked bike lanes
- Fallen branches or debris
- Closed trail entrances
- Access restriction signs
- Temporary event barricades

Photos are less useful when the issue is hard to capture visually, such as heavy traffic, poor driver behavior, bad signal timing, or a stressful crossing. In those cases, a category, severity, and short note may be more helpful.

### Photo Capture Flow

Photo submission should be offered after the basic report details are captured.

1. User selects the hazard location.
2. User selects category and severity.
3. User sees an optional **Add Photo** action.
4. User can take a new photo or attach an existing recent photo.
5. User can submit the report with or without the photo.

The photo step should feel supplemental, not mandatory. The user should always be able to skip it.

### Photo Safety

The app should encourage photos only when the user is stopped and in a safe place.

Photo guidance should be direct:

- Do not take photos while riding.
- Do not enter closed areas to get a better photo.
- Do not stand in traffic or in an active work zone.
- Do not photograph people unnecessarily.
- Do not let the photo step delay leaving an unsafe location.

For quick markers, the safer flow is to save the marker first and add a photo later only if the user can do so safely.

### Photo Quality

Photos should help reviewers or other users understand the hazard quickly.

Useful photos usually show:

- The hazard itself
- Nearby street context
- Closure or warning signs
- The affected lane, sidewalk, path, or entrance
- Enough surroundings to understand the location

The app should not ask for polished photography. A clear, practical photo is enough.

### Photo Privacy

Users should be encouraged to avoid including faces, license plates, home interiors, or other personal details when possible.

If a photo includes bystanders, private property, or sensitive information, the report can still be valid, but the app should treat the image more carefully before displaying it broadly.

### Photo Impact On Trust

A photo can make a report more credible, but it should not automatically make the report correct.

Photo-backed reports may receive stronger trust when:

- The photo clearly shows the reported hazard.
- The photo matches the selected location.
- The photo is recent.
- Other users confirm the same issue.

A photo should support the report, not replace category, severity, location, and duration.

## Ride-Based Reporting Flow

The ride-based flow should be fast and safe.

1. User opens Hazard Reporting Mode.
2. User chooses **Report Near Me**.
3. User selects a hazard category.
4. User selects severity.
5. User optionally adds movement mode, duration, note, or photo.
6. User submits the report.

For active riding, the mode should favor large controls, quick choices, and the option to save a rough marker for later review. Photos should be encouraged only when the user is stopped and can take one safely.

## Quick Marker Flow

Quick markers are useful when the user does not want to stop.

1. User marks a hazard near their current position.
2. The app saves a rough marker with time and location.
3. The user continues riding.
4. Later, the user reviews the marker and fills in category, severity, and details.

This flow makes the reporting mode safer and more realistic.

## Map-Based Reporting Flow

The map-based flow supports precise reports away from the current location.

1. User opens Hazard Reporting Mode.
2. User chooses **Select On Map**.
3. User taps a point, draws a segment, or marks an area.
4. User selects a hazard category.
5. User selects severity and affected movement modes.
6. User adds optional duration, note, or photo.
7. User submits the report.

Map-based reporting should be especially good for road closures, construction corridors, long rough-surface segments, and access restrictions. If the user is reporting from memory, a photo should remain optional and should not be expected.

## Editing Existing Reports

Users should be able to update reports when conditions change.

Possible actions:

- Confirm still present
- Mark resolved
- Change severity
- Change category
- Adjust location
- Add note
- Add photo

This is important because stale hazard data can make the game worse.

## Report Lifecycle

Each hazard should have a lifecycle.

### New

Recently submitted and not yet confirmed.

### Active

Believed to be currently relevant.

### Confirmed

Supported by multiple users or trusted review.

### Expiring

Still visible but expected to be removed soon unless reconfirmed.

### Resolved

No longer active.

### Disputed

Users disagree about whether the hazard exists, where it is, or how severe it is.

## Trust And Quality

Hazard reports affect other users, so quality matters.

Trust signals can include:

- Multiple independent confirmations
- Recent confirmation
- A clear optional photo
- Consistent reports from nearby users
- Reporter history
- Reports marked resolved by later users

The system should be comfortable with uncertainty. A single report can still be useful, but it should be treated differently from a confirmed report.

## How Reports Affect Future Games

Hazard reports should feed into game planning at different strengths based on severity and confidence.

### Hard Avoid

Used for severe hazards, closures, and confirmed access restrictions.

Effect: no objectives placed there, and routes should avoid the area.

### Soft Avoid

Used for moderate hazards or uncertain reports.

Effect: objectives are less likely to appear there, especially speed-based ones.

### Warning Only

Used for minor or mode-specific hazards.

Effect: the area can remain playable, but the game may warn the user or avoid certain challenge types.

### Mode-Specific Avoid

Used when a hazard affects one movement type more than another.

Effect: a cycling session may avoid a rough road, while a walking session may still use it.

## Hazard Impact By Game Objective

### Speed Traps

Speed traps should avoid bad surfaces, blocked lanes, poor visibility, heavy traffic, and construction areas.

### Speed Zones

Speed zones should avoid closures, rough segments, complicated crossings, crowded areas, and long construction corridors.

### Landmarks

Landmarks can remain valid if they are still safely reachable, but access routes should matter.

### Random Point Targets

Random targets should avoid active hazards more aggressively because the player may be making quick decisions.

### Unique Streets

Unique street scoring can still count streets with minor hazards, but closed or restricted streets should not be encouraged.

## Audio And Notification Tone

Hazard Reporting Mode is not a game, so the tone should be calmer and more utilitarian than gameplay.

Example confirmations:

- "Hazard marked."
- "Road closure reported."
- "Bad surface report saved."
- "Marker saved. You can finish it later."
- "This area has a reported closure."
- "Construction reported nearby."

The mode should avoid making hazard reporting feel like a reward loop. The satisfaction should come from helping improve the map.

## Safety Principles

The reporting mode should avoid encouraging unsafe behavior.

Design principles:

- The user should not need to type while moving.
- Quick reports should be possible with very few interactions.
- Detailed notes should be encouraged after stopping.
- Photo submission should be optional and encouraged only when the user is stopped.
- The mode should not encourage entering closed or dangerous areas to confirm a report.
- Emergency hazards should be handled by local emergency or public works services, not only by the app.

## Privacy Principles

Hazard reports are about places, not personal movement history.

Design principles:

- Show the hazard location, not the reporter's full route.
- Avoid exposing precise personal travel patterns.
- Let users submit reports without turning the report into a social profile moment.
- Keep optional photos focused on the hazard, not bystanders, homes, license plates, or private activity.

## Example Report Types

### Construction, Passable

"Main Street between 4th and 6th has construction cones and metal plates. It is open, but not good for a speed zone."

Suggested impact: soft avoid for all modes, hard avoid for cycling speed challenges.

### Road Closed

"Bridge entrance is blocked by barricades."

Suggested impact: hard avoid.

### Bad Surface

"The westbound side has deep potholes for two blocks."

Suggested impact: soft avoid for walking and running, hard avoid for cycling speed objectives.

### Blocked Bike Lane

"Bike lane blocked by construction signs."

Suggested impact: cycling warning or cycling avoid.

### Crowded Event

"Street market is active today."

Suggested impact: temporary avoid for random targets and speed objectives.

## Example User Scenario

A cyclist is scouting a neighborhood that is often used for game sessions.

They notice that a street is technically open, but construction has narrowed the lane and added metal plates. It would be unpleasant and distracting during a game. They open Hazard Reporting Mode, choose **Report Near Me**, select **Construction, Passable**, set severity to **Moderate**, and save it.

A few blocks later, they pass a fully closed road but do not want to stop. They tap a quick marker and keep riding.

After the ride, they open the map, select the quick marker, adjust it into a road segment, mark it as **Road Closed**, and submit it. If they safely took a photo of the closure sign while stopped, they can attach it to the report.

Future game sessions avoid placing speed zones on the construction street and avoid routing players through the closed road.

## Open Design Questions

- Should users be able to report hazards during active gameplay, or only outside a game?
- Should reports be visible immediately or only after review or confirmation?
- Should trusted users have stronger reporting weight?
- Should temporary hazards automatically expire if no one confirms them?
- Should reports be anonymous, named, or optionally attributed?
- Should photo-backed reports be reviewed differently from reports without photos?
- Should users be able to draw long hazard segments, or only tap points at first?
- Should hazard visibility differ by movement mode?
- Should the game warn users about nearby hazards before start, during play, or both?

## Summary

Hazard Reporting Mode is a practical companion to the location audio game. It gives users a way to improve the shared map without playing a timed session.

Users can report hazards by riding to them, marking their current location, saving quick markers for later, or selecting points, segments, and areas on a map.

The most important distinction is between places that are closed and places that are technically accessible but bad for gameplay. That distinction lets the game avoid unsafe or unpleasant objectives while still preserving as much of the real-world play area as possible.
