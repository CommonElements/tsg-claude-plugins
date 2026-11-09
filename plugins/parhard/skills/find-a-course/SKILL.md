---
name: find-a-course
description: Use when the user asks about a specific golf course (par, booking window, singles policy, holes, tees) or wants to look one up by name, city or state.
---
1. Call `search_courses` with the name or city (add `state` as two letters when known, `holes` for 9 or 18).
2. If several match, ask which; otherwise call `get_course` with the id.
3. Report each fact with its source and verified date. Say plainly when a fact is unverified or missing. Never fill a gap from memory. Course Rating and Slope are not served.
4. Link the course page from the result.
