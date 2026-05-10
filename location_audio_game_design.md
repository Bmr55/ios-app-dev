# Location Audio Game Design

## High Concept

This is a location-based audio game for iPhone where the player starts a timed session, moves through a real-world play area, and scores by completing fast, varied, location-driven challenges.

The game is meant to feel like a live audio scavenger hunt crossed with a time-trial racing game. The player does not need to stare at the screen while playing. Instead, the game speaks nearby opportunities, confirms completed challenges, announces score changes, and gives time updates through headphones.

The core fantasy is simple:

> Start the clock, explore the area, chase opportunities, and build the biggest score before time runs out.

## Design Goals

- Make outdoor movement feel playful, reactive, and strategic.
- Let the player compete against time without needing constant screen attention.
- Reward speed, route planning, exploration, and opportunism.
- Keep the rules understandable from audio prompts alone.
- Support sessions that feel different each time, even in the same area.
- Encourage safe play by making decisions before movement and keeping in-game prompts short.

## Core Game Loop

1. The player chooses or confirms a play area.
2. The player reviews the game length and activity mode.
3. The player manually taps **Start Game**.
4. The timer begins immediately.
5. The player moves through the area to score points from different challenge types.
6. The game announces nearby opportunities and completed scoring events.
7. New random point targets appear during the session.
8. The player chooses whether to chase speed, landmarks, streets, or random targets.
9. The game ends automatically when the elapsed time limit is reached.
10. The player receives a final score breakdown and performance summary.

## Session Format

Each game is a timed session with a fixed duration. Example session lengths could include:

- 10 minutes for a quick game
- 20 minutes for a standard game
- 45 minutes for an endurance game

The session always starts from a manual start action by the player. The clock does not begin during setup, route preview, or countdown screens.

Once started, the game is built around continuous movement and real-time choices. The player is not expected to complete every available objective. The fun comes from deciding which opportunities are worth chasing before the timer expires.

## Intended Movement Modes

The game can support multiple movement modes, but each session should be tuned around one chosen mode:

- Walking
- Running
- Cycling

Speed-based scoring should be balanced differently for each mode so that the game rewards strong performance without encouraging unsafe behavior. The game should not be framed around motor-vehicle play.

## Play Area

A session takes place inside a defined play area. The area can be a neighborhood, park, campus, downtown district, waterfront, or trail network.

The best play areas have:

- Several named streets or paths
- Multiple safe route options
- Recognizable landmarks
- A mix of open stretches and turns
- Enough density that the player can make meaningful choices during the timer

The play area should feel like a game board laid over the real world.

## Scoring Overview

The player earns points from five main activity types:

1. Speed traps
2. Speed zones
3. Landmarks
4. Random point targets
5. Unique streets

The scoring system should reward a mix of skill and exploration. A player who only chases speed should score well in some categories, but a player who combines speed, route variety, landmarks, and random targets should usually have a higher ceiling.

## Challenge Type: Speed Traps

A speed trap is a single target point in the play area. The player scores by passing through or very near the point at the highest possible speed.

### Player Experience

The game announces a nearby speed trap, giving the player a short description and directionally useful context. The player chooses whether to line up an approach and hit the trap at speed.

When the player hits the trap, the game announces the result immediately:

- Whether the trap was triggered
- The recorded speed
- The points earned
- Whether it was a personal best or session-best trap

### Design Purpose

Speed traps create short bursts of intensity. They are best placed on safe, simple stretches where the player can build momentum without needing to make sudden turns.

### Scoring Feel

Speed trap scoring should be performance-based. Faster hits earn more points, with bonus tiers for excellent runs. A weak hit should still earn something, but high scores should require deliberate setup and clean movement.

Example scoring tiers:

- Clean hit: small score
- Fast hit: medium score
- Excellent hit: large score
- Best trap of the session: bonus

## Challenge Type: Speed Zones

A speed zone is a point-to-point segment. The player scores by entering the start point, traveling to the end point, and achieving the highest possible average speed across the zone.

### Player Experience

The game announces that a speed zone is nearby and names the start and finish. Once the player enters the zone, the game confirms that the run has started. At the finish, the game announces:

- Completion
- Average speed
- Points earned
- Any bonus tier reached

### Design Purpose

Speed zones create longer, more strategic efforts than speed traps. The player needs to choose a route into the start, maintain pace, and complete the segment cleanly.

### Scoring Feel

Speed zones should reward sustained movement rather than a single burst. Longer zones should generally be worth more than short ones, but only if they are practical and safe for the chosen movement mode.

Example scoring tiers:

- Completed zone: base score
- Strong average speed: bonus
- Excellent average speed: larger bonus
- Best zone performance of the session: bonus

## Challenge Type: Landmarks

Landmarks are fixed places in the play area with a scoring radius around them. The player scores by entering the landmark radius.

### Player Experience

The game can announce nearby landmarks as optional objectives. When the player reaches one, the audio confirms the visit and awards points.

Landmarks should feel like meaningful places, not arbitrary coordinates. Examples include:

- Public art
- Trailheads
- Bridges
- Plazas
- Parks
- Historic markers
- Transit stations
- Waterfront overlooks

### Design Purpose

Landmarks reward exploration and route planning. They also give the game local flavor, making different neighborhoods feel distinct.

### Scoring Feel

Landmarks should usually be reliable points. Some can be ordinary pickups, while harder-to-reach or more distant landmarks can be worth more.

Example scoring tiers:

- Standard landmark: base score
- Outlying landmark: higher score
- Landmark chain completion: bonus
- First player visit of the session: bonus

## Challenge Type: Random Point Targets

Random point targets are temporary scoring opportunities that appear inside the play area during the session.

### Player Experience

At random intervals, the game announces that a new target has appeared. The player hears enough context to decide whether to chase it. The target remains available for a limited time.

When the player reaches the target radius, the game confirms collection and awards points. If the target expires, the game may briefly announce that it is gone.

### Design Purpose

Random targets keep the session dynamic. They interrupt over-planned routes, create risk-reward decisions, and make repeated playthroughs less predictable.

### Scoring Feel

Random targets should be worth enough to tempt the player, but not so much that ignoring other scoring types becomes optimal.

Example scoring rules:

- Nearby target: modest score
- Farther target: higher score
- Fast collection: bonus
- Consecutive random targets collected: streak bonus
- Expired target: no score

## Challenge Type: Unique Streets

The player earns points by moving onto as many unique named streets, paths, or trails as possible during the session.

### Player Experience

The game can occasionally announce street milestones:

- First new street
- Five unique streets
- Ten unique streets
- New street streak
- Rare or high-value street reached

The player does not need to stop on a street. Simply entering and traveling along a new named street should feel like progress.

### Design Purpose

Unique street scoring rewards exploration instead of pure speed. It gives the player a reason to weave through the area, discover side streets, and avoid repeating the same loop.

### Scoring Feel

The first set of unique streets should be easy and satisfying. Later streets should become more valuable or unlock milestone bonuses so the player is encouraged to keep exploring.

Example scoring structure:

- Each new street: base score
- Every fifth unique street: milestone bonus
- Street variety streak: bonus
- Repeated street: no additional score

## Score Balance

A good final score should usually come from combining multiple behaviors:

- Hitting speed traps when nearby
- Running or riding speed zones when the route lines up
- Detouring to landmarks when efficient
- Chasing random targets when the reward justifies the time
- Choosing routes that maximize unique streets

No single scoring type should completely dominate the game. The player should constantly be asking:

> Is this opportunity worth the time it will cost?

## Risk And Reward

The game should create interesting choices without requiring complicated rules.

Examples:

- A speed trap may be close, but facing the wrong direction.
- A landmark may be easy points, but it pulls the player away from a speed zone.
- A random target may be valuable, but it expires soon.
- A dense street grid may offer unique street points, but slower movement.
- A long straightaway may help speed scoring, but offers fewer exploration points.

The best sessions should produce small regrets and exciting recoveries: missed targets, last-minute bonuses, unexpected shortcuts, and final pushes before the clock runs out.

## Audio Experience

The game is audio-first during active play. Spoken prompts should be short, timely, and useful.

### Audio Prompt Types

- Start countdown
- Timer milestones
- Nearby opportunity alerts
- Challenge start confirmations
- Challenge completion results
- Score updates
- Random target arrivals
- Random target expirations
- Streak and milestone announcements
- Final countdown
- Game over summary

### Audio Tone

The voice should feel energetic but not frantic. It should help the player make decisions without overwhelming them.

Example prompt styles:

- "Speed trap ahead. Line it up if you want it."
- "Trap hit. Twenty-two points."
- "Speed zone started."
- "Zone complete. Strong average. Forty points."
- "Landmark collected."
- "New target appeared nearby. Two minutes left."
- "Ten unique streets. Exploration bonus."
- "One minute remaining."

## Time Pressure

The timer is the central constraint. Every objective competes against the same remaining time.

The game should give clear time updates at important moments:

- Game start
- Halfway point
- Five minutes remaining
- Two minutes remaining
- One minute remaining
- Final thirty seconds
- Final ten seconds
- Game over

In shorter sessions, time prompts should be less frequent so they do not crowd out gameplay audio.

## End Of Game

The game ends automatically when the elapsed time limit is reached. The final result should be immediate and satisfying.

The post-game summary should include:

- Final score
- Rank or rating
- Best speed trap
- Best speed zone
- Landmarks collected
- Random targets collected
- Unique streets reached
- Biggest bonus
- Missed opportunities, if useful

The player should be able to understand what kind of run they had:

- Fast but narrow
- Exploratory
- Opportunistic
- Balanced
- Landmark-heavy
- Street-grid specialist

## Session Ratings

Session ratings give the player a quick emotional read on the final result.

Example ratings:

- Warmup
- Solid Run
- Hot Streak
- Route Master
- City Legend

Ratings should be based on the whole score, not just speed. A slower but clever route should still be able to feel successful.

## Progression Ideas

Progression should deepen replayability without making new players feel underpowered.

Possible progression systems:

- Personal bests by play area
- Best speed trap records
- Best speed zone records
- Landmark collection history
- Unique street count records
- Daily or weekly challenge areas
- Session badges
- Streaks for playing different neighborhoods

Progression should celebrate mastery, consistency, and exploration rather than only rewarding raw speed.

## Game Variants

### Sprint

A short session focused on high-value speed traps, random targets, and fast decisions.

### Explorer

A medium session where landmarks and unique streets are worth more.

### Circuit

A session built around several speed zones that can be chained together efficiently.

### Landmark Hunt

A session where landmark collection drives most of the score.

### Chaos Run

A session with frequent random target appearances and heavier streak bonuses.

## Multiplayer And Social Possibilities

The core design works as a solo timed game, but it can later support social competition.

Possible social features:

- Leaderboards by play area
- Friend challenges
- Ghost score challenges
- Weekly neighborhood events
- Team score totals
- Category-specific records

Social competition should compare similar movement modes so walking, running, and cycling scores remain meaningful.

## Safety Principles

The game should be exciting without demanding unsafe behavior.

Design principles:

- The game should not require screen interaction during active movement.
- Prompts should be short enough to understand quickly.
- Speed challenges should be placed where the player can move safely.
- The game should not encourage play while driving.
- The player should be reminded to follow local laws and stay aware of surroundings.
- Speed scoring should be tuned to the selected movement mode.
- The game should avoid sudden audio that could startle the player.

The player should feel challenged, not pressured into reckless choices.

## Example Session Narrative

The player starts a 20-minute cycling session in a neighborhood grid.

At the start, they hear a countdown and begin moving. A speed trap is nearby, so they turn onto a straight street and hit it for an early burst of points. The game announces a solid trap score.

A random target appears two blocks away. The player decides it is worth chasing because it also passes two unvisited streets. On the way, they collect unique street points, then reach the target before it expires.

Halfway through the session, the player hears that a speed zone is nearby. They line up the start, enter the zone, and push through to the finish. The average speed earns a strong bonus.

With five minutes left, they switch strategy and start collecting landmarks. One landmark is close, but another would pull them too far from the final cluster of streets. They choose the streets.

In the final minute, a random target appears nearby. The player makes a last push, collects it with seconds left, and the game ends automatically.

The final summary calls out a balanced run: strong speed zone, twelve unique streets, three landmarks, and two random targets.

## Open Design Questions

- What is the primary first movement mode: walking, running, or cycling?
- Should each play area have a recommended session length?
- Should speed traps and speed zones be visible before the game starts?
- Should random targets appear based on time, score, player location, or pure randomness?
- How much should the game reveal through audio versus an optional map?
- Should players be able to pause, or should each session be a continuous timed run?
- Should landmark scoring favor famous places, useful route anchors, or hidden discoveries?
- Should unique street scoring include trails, paths, alleys, and park roads?
- Should the final score compare against friends, personal history, or a global leaderboard?

## Summary

This game turns a real-world area into a timed audio challenge. The player starts manually, the clock begins, and every movement decision becomes a scoring choice.

Speed traps reward quick bursts. Speed zones reward sustained pace. Landmarks reward exploration. Random targets add surprise. Unique streets reward route variety.

The strongest version of the game is not just about going fast. It is about reading the area, choosing smart detours, reacting to new opportunities, and finishing the session with a route that feels like it could only have happened in that place.
