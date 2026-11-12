---
name: courses-near-me
description: Use when the user wants golf courses near a town, address or their location.
---
1. Turn the place into latitude and longitude (ask if ambiguous).
2. Call `find_courses_near` with `radiusMi` (25 by default, up to 250) and a small `limit`.
3. For the ones the user cares about, call `get_facility` and report facts with source and date.
4. Say how many places the radius holds if the result is truncated.
