# Fuiz Quiz Format Specification

> Reference document for Claude sessions working on this repository.
> Last updated: 2026-01-29
> Sources: https://github.com/fuiz/game (Rust backend), https://github.com/fuiz/website (TypeScript frontend)

## Overview

Fuiz is an open-source quiz tool with quizzes stored in TOML format (`config.toml`). This document describes the complete specification for creating compatible quiz files.

---

## Root Structure

```toml
title = "Quiz Title"    # Required, max 200 characters

[[slides]]              # Array of slides, max 100 slides
[slides.SlideType]      # Tagged enum: MultipleChoice | TypeAnswer | Order
# ... slide fields
```

---

## Slide Types

### 1. MultipleChoice

Standard multiple choice questions with predefined answer options.

```toml
[[slides]]
[slides.MultipleChoice]
title = "Question text?"                    # Required, max 200 chars
media.Image.Url.url = "image.jpg"           # Optional media
media.Image.Url.alt = "Alt text"            # Required if media present, max 200 chars
introduce_question = 5000                   # Delay before showing answers (ms)
time_limit = 30000                          # Time to answer (ms)
points_awarded = 1000                       # Max points for correct answer
answers = [
    { content.Text = "Answer 1", correct = true },
    { content.Text = "Answer 2", correct = false },
    { content.Text = "Answer 3", correct = false },
    { content.Text = "Answer 4", correct = false },
]
```

**Answer format:**
- `content.Text = "string"` - Text answer (max 200 chars)
- `content.Image.Url.url = "..."` - Image answer (see Media Types)
- `correct = true|false` - Whether this is a correct answer

**Constraints:**
- Min 1, max 8 answers
- Multiple correct answers allowed

---

### 2. TypeAnswer

Free text response questions where users type their answer.

```toml
[[slides]]
[slides.TypeAnswer]
title = "What is the capital of France?"    # Required, max 200 chars
media.Image.Url.url = "france.jpg"          # Optional
media.Image.Url.alt = "Map of France"
introduce_question = 5000
time_limit = 30000
points_awarded = 1000
answers = ["Paris", "paris", "PARIS"]       # Accepted answers
case_sensitive = false                      # Case matching behavior
```

**Constraints:**
- Min 1, max 16 accepted answers
- Each answer max 200 characters
- `case_sensitive`: when false, comparison ignores case

---

### 3. Order

Ranking/sequencing questions where users arrange items in correct order.

```toml
[[slides]]
[slides.Order]
title = "Order these planets from the Sun"  # Required, max 200 chars
media.Image.Url.url = "solar-system.jpg"    # Optional
media.Image.Url.alt = ""
introduce_question = 5000
time_limit = 45000
points_awarded = 1000
answers = ["Mercury", "Venus", "Earth", "Mars"]  # Items in CORRECT order
axis_labels = { from = "Closest", to = "Farthest" }  # Optional labels
```

**Constraints:**
- Min 1, max 8 items in answers array
- Each item max 100 characters
- `axis_labels.from` and `axis_labels.to` each max 100 chars
- Items are shuffled for display; user must restore correct order

---

## Media Types

Media is optional on any slide. Three image source types supported:

### URL (External/Local Reference)
```toml
media.Image.Url.url = "https://example.com/image.jpg"
media.Image.Url.alt = "Description for accessibility"
```
- `url`: Path or URL to image file
- `alt`: Alt text, max 200 chars

### Corkboard (Fuiz Hosting Service)
```toml
media.Image.Corkboard.id = "abc123def456ghij"
media.Image.Corkboard.alt = "Description"
```
- `id`: Exactly 16 characters, references uploaded image
- `alt`: Alt text, max 200 chars

### Base64 (Inline Encoded)
```toml
media.Image.Base64.data = "data:image/png;base64,iVBORw0KGgo..."
media.Image.Base64.alt = "Description"
media.Image.Base64.hash = "optional-dedup-hash"
```
- `data`: Full data URI with base64 content
- `alt`: Alt text, max 200 chars
- `hash`: Optional, used for deduplication on export

### Supported Image Formats
APNG, AVIF, GIF, JPEG, PNG, SVG, WebP

---

## Timing Values

### introduce_question (delay before showing answers)
Allowed values in milliseconds:
- `0` (no delay)
- `3000` (3 seconds)
- `5000` (5 seconds)
- `7000` (7 seconds)
- `10000` (10 seconds)
- `15000` (15 seconds)
- `30000` (30 seconds) - backend max

### time_limit (answer window)
Allowed values in milliseconds:
- `5000` (5 seconds) - backend min
- `10000` (10 seconds)
- `20000` (20 seconds)
- `30000` (30 seconds)
- `60000` (1 minute)
- `120000` (2 minutes)
- `240000` (4 minutes) - max

### points_awarded
Common values: `0`, `500`, `1000`, `2000`

**Scoring behavior:** Points decrease linearly from `points_awarded` to 50% over the `time_limit`. Faster correct answers earn more points.

---

## Complete Constraints Reference

| Field | Constraint |
|-------|-----------|
| Quiz `title` | Max 200 characters |
| Slide `title` | Max 200 characters |
| Total `slides` | Max 100 |
| `introduce_question` | 0-30 seconds |
| `time_limit` | 5-240 seconds |
| **MultipleChoice** | |
| └ answers | 1-8 items |
| └ answer text | Max 200 chars |
| **TypeAnswer** | |
| └ answers | 1-16 items |
| └ each answer | Max 200 chars |
| **Order** | |
| └ answers | 1-8 items |
| └ each item | Max 100 chars |
| └ axis_labels.from/to | Max 100 chars each |
| **Media** | |
| └ Corkboard id | Exactly 16 chars |
| └ alt text | Max 200 chars |

---

## Export Behavior

When exporting from Fuiz:
- **No Base64 images** → Single `.toml` file named `{title}.toml`
- **With Base64 images** → ZIP archive named `{title}.zip` containing:
  - `config.toml` - Quiz definition
  - Image files with hash-based names (e.g., `4aae2cef821d9044d1854013e8fdf5cb46582a10.jpg`)

---

## Example: Complete Quiz

```toml
title = "Geography Quiz"

[[slides]]
[slides.MultipleChoice]
title = "What is the capital of France?"
introduce_question = 5000
time_limit = 20000
points_awarded = 1000
answers = [
    { content.Text = "Paris", correct = true },
    { content.Text = "London", correct = false },
    { content.Text = "Berlin", correct = false },
    { content.Text = "Madrid", correct = false },
]

[[slides]]
[slides.MultipleChoice]
title = "Which country is this flag from?"
media.Image.Url.url = "japan-flag.png"
media.Image.Url.alt = "A red circle on white background"
introduce_question = 3000
time_limit = 15000
points_awarded = 500
answers = [
    { content.Text = "Japan", correct = true },
    { content.Text = "China", correct = false },
    { content.Text = "South Korea", correct = false },
]

[[slides]]
[slides.TypeAnswer]
title = "Name the longest river in the world"
introduce_question = 5000
time_limit = 30000
points_awarded = 1000
answers = ["Nile", "The Nile", "Nile River"]
case_sensitive = false

[[slides]]
[slides.Order]
title = "Order these countries by population (largest first)"
introduce_question = 5000
time_limit = 45000
points_awarded = 2000
answers = ["China", "India", "United States", "Indonesia"]
axis_labels = { from = "Largest", to = "Smallest" }
```

---

## Online Quiz Metadata (Optional)

When publishing to Fuiz online platform, additional metadata is supported:

```toml
author = "Quiz Author Name"
language = "en"                    # Language code
subjects = ["Geography", "History"]
grades = ["Secondary-School"]      # University, Secondary-School, Primary-School, Other
```

### Available Subjects
Art, Business, Computer Science, Culture and Traditions, English Language Arts, Finance, General Knowledge, Geography, History, Languages, Law, Math, Music, Science, Seasonal, Social Emotional Learning, Social Studies, Trivia

---

## Technical Notes

- **Serialization:** Uses `serde` (Rust) / `@ltd/j-toml` (TypeScript)
- **Validation:** Backend uses `garde` crate for constraint validation
- **Runtime states:** Slides progress through: Unstarted → Question → Answers → AnswersResults
- **Max players:** 1000 per game session

---

## Repository Links

- Backend (Rust): https://github.com/fuiz/game
- Frontend (TypeScript): https://github.com/fuiz/website
- GitLab mirror: https://gitlab.com/fuiz/game
