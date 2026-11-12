<p align="center">
  <img src="assets/logo.png" alt="Sports Manager logo" width="140">
</p>

<h1 align="center">Sports Manager</h1>

<p align="center">
  <img src="https://img.shields.io/badge/version-1.0--SNAPSHOT-blue" alt="Version 1.0-SNAPSHOT">
  <img src="https://img.shields.io/badge/Java-21-orange" alt="Java 21">
  <img src="https://img.shields.io/badge/JavaFX-21.0.2-informational" alt="JavaFX 21.0.2">
  <img src="https://img.shields.io/badge/platform-Windows%20%7C%20macOS%20%7C%20Linux-lightgrey" alt="Platform">
</p>

A turn-based sports management game built with Java 21 and JavaFX 21. Manage a football or handball team through a full league season — set tactics, pick your lineup, simulate matches period by period, train your squad, and track standings.

**Course:** CE216  
**Team:** Dining Philosophers

---

## Features

- Two fully supported sports: **Football** and **Handball**
- Period-by-period match simulation with a live event log (goals, cards, injuries, 7-metre throws, penalties)
- Tactic and formation system with attack/defense multipliers
- Squad management with injury and suspension tracking
- Weekly training system with coach speciality bonuses
- Full league with auto-simulated AI fixtures and live standings
- JSON save/load — everything persists between sessions
- 27 unit tests covering core systems

---

## Requirements

| Component | Version |
|-----------|---------|
| JDK | 21 |
| JavaFX | 21.0.2 |
| Maven | 3.8+ |
| OS | Windows / macOS / Linux |

---

## Running the App

```bash
mvn javafx:run
```

Running the tests:

```bash
mvn test
```

All 27 tests should pass. They cover the injury system, GameSession singleton, team validation, league standings, and the match engine.

---

## Project Structure

```
src/
├── main/
│   ├── java/com/sportsmanager/
│   │   ├── core/
│   │   │   ├── engine/       # MatchEngine interface, SegmentResult
│   │   │   ├── factory/      # SportFactory, SportRegistry
│   │   │   └── model/        # Person, Player, Coach, Team, GameSession, Sport, Tactic, ...
│   │   ├── football/         # FootballPlayer, FootballTeam, FootballMatchEngine, FootballFactory, ...
│   │   ├── handball/         # HandballPlayer, HandballTeam, HandballMatchEngine, HandballFactory, ...
│   │   ├── league/           # Fixture, MatchDay
│   │   ├── ui/controller/    # One JavaFX controller per screen (13 screens)
│   │   └── util/             # GameSaveManager, TeamDataLoader, TeamLogoHelper
│   └── resources/com/sportsmanager/ui/
│       ├── *.fxml            # 14 FXML layout files
│       ├── styles.css
│       └── icons/            # Match event icons (ball, cards, band-aid, ...)
└── test/java/                # 6 JUnit 5 test classes
```

---

## Design Patterns

| Pattern | Where | Purpose |
|---------|-------|---------|
| Singleton | `GameSession` | One object holds all active game state; every controller reads from it |
| Abstract class | `Person`, `Player`, `Coach`, `Team` | Shared logic written once, reused by both sport subclasses |
| Factory | `SportFactory` / `SportRegistry` | Creates sport-specific objects without the caller knowing which sport is active |
| Template Method | `Coach.conductTraining()` | The training flow is fixed; only the attribute bonuses differ per sport |
| Strategy | `Tactic` | Each formation carries its own attack and defense multipliers |

---

## Screens

| Screen | Description |
|--------|-------------|
| Splash | Animated intro; skip by clicking |
| Sport Selection | Choose Football or Handball; start a new game or load a save |
| Team Selection | Pick your team from the full roster list |
| Dashboard | Central hub — shows season, league position, next fixture; links to all other screens |
| Squad | Full squad list with OVR, goals, yellow/red cards, appearances, injury/suspension status |
| Tactics | Choose a formation and view the pitch diagram |
| Lineup | Select your starting lineup; invalid selections are rejected |
| Match | Period-by-period simulation with a live event log and half-time tactics change |
| Schedule | Full fixture list; played matches show scores; your rows are highlighted |
| League Table | Live standings with P/W/D/L/GF/GA/GD/Pts |
| Training | Weekly training programs; coach speciality gives a 1.5x bonus |
| End of Season | Final standings, player OVR changes, option to start a new season |
| Saves | List, load, or delete saved games |

---

## Sports at a Glance

|  | Football | Handball |
|--|----------|----------|
| Lineup | 11 players | 7 players |
| Periods | 2 × 45 min | 2 × 30 min |
| Positions | GK, DEF, MID, FWD | GK, WING, BACK, PIV |
| Formations | 4-4-2 / 4-3-3 / 4-2-3-1 / 5-3-2 | 4-2 / 3-2-1 / 6-0 |
| Attributes | speed, shooting, passing, ballControl, defending, physicality | throwing, speed, agility, jumping, defending, stamina |
| Special events | Penalty kicks | 7-metre throws, 2-min suspensions, disqualifications |
| Cards | Yellow (2nd = auto red), Red | Yellow (warning only), Disqualification (2-match ban) |

---

## Save Files

Saves are written to `~/SportsManagerSaves/<name>.json` and include player attributes, injury/suspension counters, stats, lineup, tactic, league standings, and session metadata. Only saves matching the currently selected sport appear in the load list.

---

## Dependencies

| Library | Version | Use |
|---------|---------|-----|
| JavaFX | 21.0.2 | UI framework |
| Gson | 2.10.1 | JSON save/load and team data parsing |
| JUnit 5 | 5.10.2 | Unit tests |
