# Data Model

This document defines the schema used by the Mountain Project Recommender.

## Tables

### users
Represents an application user.
- id: UUID (primary key)
- email_hash: text (hashed user email for privacy)
- created_at: timestamptz

### routes
Represents a climbing route or boulder problem.
- id: UUID (primary key)
- name: text (e.g. "Bob's Mantle")
- grade_raw: text (e.g. "5.10a" or "V4")
- rating_code: int (MP codes for rating difficulty internally, translation to grades found at: https://www.mountainproject.com/forum/topic/116208707/rating-code-sorting-ticks)
- avg_stars: float
- type: enum (sport, trad, boulder, alpine, ice)
- area_path: text[] (hierarchy, e.g. ["Rhode Island","Lincoln Woods","Sit Down Area / Druid Circle","Split Rock"])
- pitches: int
- URL: text 

### ticks
Represents a user’s recorded ascent (tick).
- id: UUID (primary key)
- user_id: UUID (FK → users.id)
- route_id: UUID (FK → routes.id)
- date: date
- style: enum (attempt, flash, send)
- usr_stars: integer (-1–4 user rating, -1= unranked, 0 = bomb, 1-4 for stars)
- usr_grade_raw: text
- usr_rating_code: int
- notes: text

### recs_cache
Stores cached recommendations for quick retrieval.
- user_id: UUID (FK → users.id)
- generated_at: timestamptz
- payload_json: jsonb
