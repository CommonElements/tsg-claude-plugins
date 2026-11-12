---
name: plan-a-golf-trip
description: Use when the user is choosing a state or region for a golf trip or outing and wants coverage and candidate courses.
---
1. Call `list_states` to see how many places are listed in each candidate state.
2. Use `search_courses` with `state` and `holes`, or `find_courses_near` around a hub city.
3. Compare only facts that have a source; list unknowns separately.
4. For a group game, call `list_game_formats`. Games are points only; never suggest wagering.
