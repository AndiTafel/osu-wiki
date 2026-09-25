---
tags:
  - quickplay
  - matchmaking
  - match making
  - multiplayer
  - MMR
  - rating
---

# Quick play

**Quick play** was a queue-based multiplayer mode introduced to [osu!(lazer)](/wiki/Client/Release_stream/Lazer) on October 29, 2025 ([release](https://osu.ppy.sh/home/changelog/lazer/2025.1029.1)). It was the first queue-based multiplayer system introduced to the game.

On April 18, 2026 ([release](https://osu.ppy.sh/home/changelog/lazer/2026.418.0)), this mode was replaced by [Ranked play](/wiki/Gameplay/Ranked_play) and is no longer available.

## Interface

Quick play is accessible at `play > multi > quick play` via the main menu. The traditional multiplayer lobby interface is accessible via the `lounge` option.

![Main menu interface](img/quick-play-main-menu.png "Main menu interface")

To queue for a match, first select a [mode](/wiki/Game_mode)[^mania], then click the button to queue for a match.

![Mode selection](img/quick-play-queue-panel.png "Mode selection")

After entering the queue, players are brought to a waiting area. This screen shows who else is queueing simultaneously. Player avatars appear as moving bubbles that group together as lobbies are formed.

This screen can be exited while continuing to queue for matches in the background by clicking the "Queue in background" button.

![Waiting area](img/quick-play-queue-screen.png "Waiting area")

When a lobby is formed and all players confirm to join, the players land on a beatmap selection screen where they can vote on the first beatmap to play from a [predetermined pool of maps](#beatmap-curation).

![Beatmap selection screen](img/quick-play-beatmap-selection-screen.png "Beatmap selection screen")

## Gameplay

Lobbies are formed of up to 8 players of similar skill level. The matches last for 5 rounds.

### Beatmap selection

During each round, players are presented a small set of beatmaps. Clicking on a beatmap casts a vote for it, where multiple votes on a map increase the likelihood of it being picked. After all players have cast their vote, one beatmap will be picked at random to be played for that round.

### Standings

Matchmaking rating ("MMR" or "rating") is a hidden variable assigned to each player, which fluctuates depending on how well they perform in quick play lobbies. MMR is used to group players of similar skill levels together when forming lobbies. Curated beatmaps are assigned fixed MMR values, which enables the server to more accurately estimate whether a beatmap is too easy or difficult for a given lobby.

A beatmap's MMR is directly derived from its [star rating](/wiki/Beatmap/Star_rating) (SR). Beatmap curators have the ability to manually specify an override SR to better inform the server of a beatmap's true difficulty. For example, due to the reading requirement, [this beatmap](https://osu.ppy.sh/beatmapsets/1799413#osu/3688768)'s difficulty (as judged by the curators) is approximately 7.7 stars despite only being ~5.5 stars.

### Scoring

A user's standing is dependent on the amount of points they earn throughout the match. A better score in each round will award more points following the table below:

| Placement | Points |
| :-- | --: |
| 1st | 15 |
| 2nd | 12 |
| 3rd | 10 |
| 4th | 8 |
| 5th | 6 |
| 6th | 4 |
| 7th | 2 |
| 8th | 1 |

## Matchmaking rating

Matchmaking rating ("MMR", "rating") is a variable assigned to each user which fluctuates depending on how well they perform in quick play matches. Ratings are used to group players of similar skill levels together when forming matches.

Beatmaps are also rated according to the level which they are appropriate to be played at such that the beatmap pool for each match should be representative of the average player skill level.

## Beatmap curation

Beatmaps used in quick play are currently curated by a team of community volunteers. The full list of available beatmaps for all modes is publicly available via [this spreadsheet](https://docs.google.com/spreadsheets/d/1ZbvLvHlXH3IF1WgN4YkHqOttO7wG-1Duto-535tqKnQ/edit?gid=0#gid=0).

Each game mode has a mappool curated by community members who are familiar with beatmaps of the mode. All beatmaps featured are either Ranked, Approved, or Loved.

[^mania]: osu!mania 4K and 7K are split into separate queues and function as completely separate modes in the context of quick play.
