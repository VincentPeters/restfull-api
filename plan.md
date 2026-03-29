# Companion Website: RESTful API Cursus

## Context

Students at KdG need an interactive companion site to reinforce REST API concepts taught in class. The course is in Dutch, covers REST principles, URI naming, and API clients. Students already have a Gaming API to test with (`https://kdg-gaming-api.me-945.workers.dev`). The site needs to be static (GitHub Pages / Cloudflare) and engaging.

## Tech Stack

- **Astro** — static site generator, fast, minimal JS by default
- **Vanilla JS** — interactive components via Astro `client:load` islands
- **Tailwind CSS** — rapid styling, consistent design
- **GitHub Pages or Cloudflare Pages** — deployment target

## Project Structure

```
companion-site/
├── astro.config.mjs
├── package.json
├── tailwind.config.mjs
├── public/
│   └── favicon.svg
├── src/
│   ├── layouts/
│   │   └── BaseLayout.astro          # Shared layout (nav, footer)
│   ├── components/
│   │   ├── Navigation.astro           # Sidebar/top nav
│   │   ├── ExerciseCard.astro         # Reusable exercise wrapper
│   │   ├── FeedbackMessage.astro      # Correct/incorrect feedback
│   │   ├── uri-builder/
│   │   │   └── UriBuilder.astro       # URI construction exercise
│   │   ├── uri-spotter/
│   │   │   └── UriSpotter.astro       # Good vs Bad URI picker
│   │   ├── quiz/
│   │   │   └── RestQuiz.astro         # REST principles quiz
│   │   ├── json-explorer/
│   │   │   └── JsonExplorer.astro     # Interactive JSON viewer
│   │   ├── architecture/
│   │   │   └── ArchitectureBuilder.astro  # Drag-and-drop layers
│   │   └── glossary/
│   │       └── GlossarySearch.astro   # Searchable glossary
│   ├── data/
│   │   ├── uri-challenges.json        # URI builder exercise data
│   │   ├── uri-spotter.json           # Good/bad URI pairs
│   │   ├── quiz-questions.json        # REST principles quiz data
│   │   ├── architecture-layers.json   # Layer ordering data
│   │   └── glossary.json              # Terms and definitions
│   ├── pages/
│   │   ├── index.astro                # Homepage / dashboard
│   │   ├── oefeningen/
│   │   │   ├── uri-builder.astro      # URI Builder Challenge
│   │   │   ├── uri-spotter.astro      # Good vs Bad URI Spotter
│   │   │   ├── rest-quiz.astro        # REST Principles Quiz
│   │   │   ├── json-explorer.astro    # JSON Explorer
│   │   │   └── architectuur.astro     # Architecture Diagram Builder
│   │   └── naslagwerk/
│   │       └── index.astro            # Glossary & Cheat Sheet
│   └── styles/
│       └── global.css                 # Global styles + Tailwind
```

## Pages & Features

### 1. Homepage (`/`)

- Welcome message, course overview
- Card grid linking to each exercise with icons and descriptions
- Link to the Gaming API docs
- Progress indicator (localStorage-based)

### 2. URI Builder Challenge (`/oefeningen/uri-builder`)

- Shows a scenario in Dutch, e.g.: _"Haal alle games op die tot het genre 'rpg' behoren"_
- Student types/builds the URI in an input field
- Validates against the correct answer with helpful hints
- Uses the Gaming API endpoints as context (games, developers, reviews)
- ~15 challenges covering all 8 URI naming rules
- Difficulty levels: beginner, intermediate, advanced

**Data structure (`uri-challenges.json`):**

```json
[
  {
    "id": 1,
    "difficulty": "beginner",
    "scenario": "Haal alle games op uit de API.",
    "correctUri": "/games",
    "hints": ["Gebruik meervoudsvorm", "Geen werkwoorden"],
    "rule": "Gebruik zelfstandige naamwoorden, geen werkwoorden",
    "explanation": "Een collectie van games wordt aangesproken met /games"
  }
]
```

### 3. Good vs Bad URI Spotter (`/oefeningen/uri-spotter`)

- Shows two URI options side-by-side
- Student picks the RESTful one
- Explains which naming rule applies
- Score tracker per session
- ~20 pairs covering all naming rules

**Data structure (`uri-spotter.json`):**

```json
[
  {
    "id": 1,
    "good": "/games?genre=rpg",
    "bad": "/getRpgGames",
    "rule": "Gebruik zelfstandige naamwoorden + query parameters voor filtering",
    "explanation": "Filtering hoort via query parameters, niet in het pad"
  }
]
```

### 4. REST Principles Quiz (`/oefeningen/rest-quiz`)

- Scenario-based multiple choice questions
- Each question describes a situation, student identifies which REST principle applies or is violated
- Immediate feedback with explanation referencing course material
- Covers all 5 principles with IoT/gaming examples
- ~20 questions, randomized order

**Data structure (`quiz-questions.json`):**

```json
[
  {
    "id": 1,
    "scenario": "Een server slaat de login-sessie op en gebruikt die bij het volgende verzoek zonder dat de client een token meestuurt.",
    "question": "Welk REST-principe wordt hier geschonden?",
    "options": [
      "Uniform Interface",
      "Client-Server",
      "Stateless",
      "Cacheable"
    ],
    "correctIndex": 2,
    "explanation": "Bij REST is elke request stateless — de server bewaart geen sessie-informatie."
  }
]
```

### 5. JSON Explorer (`/oefeningen/json-explorer`)

- Fetches live data from the Gaming API (`GET /games`, `GET /developers`)
- Renders JSON in a collapsible tree view
- Challenges: "Wat is de titel van het eerste spel?", "Hoeveel developers komen uit Japan?"
- Students click on the correct JSON path/value
- Teaches JSON navigation and API response structure

### 6. Architecture Diagram Builder (`/oefeningen/architectuur`)

- Shows shuffled architecture layers (Client, Load Balancer, API Gateway, Auth Server, Application Server, Database)
- Students drag them into the correct order (top to bottom)
- Visual arrows connect the layers when placed correctly
- Explanation panel shows what each layer does
- Based on the Layered System example from the course

### 7. Glossary & Cheat Sheet (`/naslagwerk`)

- Searchable list of terms (API, REST, URI, HTTP methods, status codes, etc.)
- HTTP Methods reference table (GET, POST, PUT, PATCH, DELETE)
- Status codes reference (200, 201, 400, 401, 404, 500)
- URI naming rules summary
- REST principles quick reference
- Link to the Gaming API docs

## Implementation Plan (Build Order)

### Step 1: Project Setup

- Initialize Astro project with Tailwind CSS
- Create base layout, navigation, global styles
- Set up deployment config (GitHub Pages adapter)
- Create homepage with card grid

### Step 2: Data Files

- Create all JSON data files with exercise content in Dutch
- Reference Gaming API endpoints throughout

### Step 3: Glossary & Cheat Sheet

- Build the reference page (simplest, no complex interactivity)
- Searchable term list, HTTP methods table, status codes

### Step 4: URI Spotter (Good vs Bad)

- Two-option picker component
- Score tracking, feedback, rule explanations
- Simplest interactive exercise

### Step 5: REST Principles Quiz

- Multiple choice component
- Randomized questions, score, explanations

### Step 6: URI Builder Challenge

- Text input with validation
- Hint system, difficulty levels
- Pattern matching for flexible answer checking

### Step 7: JSON Explorer

- Fetch from Gaming API (live data)
- Collapsible JSON tree renderer
- Challenge questions with clickable paths

### Step 8: Architecture Diagram Builder

- Drag-and-drop (vanilla JS, HTML Drag and Drop API)
- Layer ordering validation
- Visual connections and explanations

### Step 9: Polish & Deploy

- Progress tracking (localStorage)
- Responsive design check
- Deploy to GitHub Pages or Cloudflare

## Design System — "REST Redactie" (Editorial Workshop)

**Concept**: A beautifully typeset technical journal. Each exercise feels like a magazine article — generous whitespace, strong typographic hierarchy, warm tones. Sophisticated enough to make first-year students feel like real developers, approachable enough to never intimidate.

**What makes it unforgettable**: Code and URIs are treated like *specimens* — displayed with the reverence of a museum exhibit. Every endpoint, every JSON response is a carefully composed visual element, not just text in a box.

### Typography
- **Display**: [Fraunces](https://fonts.google.com/specimen/Fraunces) — variable optical-size serif, warm and characterful. Used for page titles and section headers.
- **Body**: [DM Sans](https://fonts.google.com/specimen/DM+Sans) — geometric sans-serif, clean and modern. Used for explanations, instructions, UI elements.
- **Code**: [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono) — ligatures, clear distinction between similar characters. Used for URIs, JSON, code snippets.
- **Scale**: Fluid sizing with `clamp()` — `clamp(1rem, 0.9rem + 0.5vw, 1.125rem)` for body, larger jumps for headings.

### Color Palette (warm, editorial)
```css
:root {
  --bg-primary:    #F7F3EE;  /* warm cream — main background */
  --bg-secondary:  #EDE8E1;  /* slightly darker cream — exercise areas */
  --bg-code:       #2D2A26;  /* deep warm charcoal — code blocks (inverted) */
  --text-primary:  #2D2A26;  /* charcoal — body text */
  --text-secondary:#6B6560;  /* warm gray — supporting text */
  --accent-rust:   #C4573A;  /* terracotta — primary accent, links, active states */
  --accent-sage:   #5B7B6A;  /* sage green — success, correct answers */
  --accent-ochre:  #D4A843;  /* warm gold — highlights, badges, hints */
  --error:         #C4573A;  /* terracotta doubles as error (contextual) */
  --success:       #5B7B6A;  /* sage green for correct feedback */
  --border:        #D6D0C8;  /* warm border color */
}
```

No pure black. No pure white. Every neutral is tinted warm.

### Layout Principles
- **Asymmetric magazine grid**: Content doesn't always center. Left-aligned text with generous right margins creates a reading rhythm.
- **Pull quotes**: Key REST principles displayed as large typographic callouts in the margin.
- **No card-in-card nesting**: Exercises use subtle background shifts and generous padding, not nested containers.
- **Specimen display**: URIs and JSON rendered in dark code blocks that feel like carefully mounted exhibits — generous internal padding, slight inset shadow, no heavy borders.
- **Whitespace as structure**: Sections separated by space, not lines or dividers.

### Navigation
- **Top bar**: Minimal — site title (Fraunces, large) + flat text links to sections.
- **No sidebar**: Keep the magazine feel. Exercises are full-width experiences.
- **Breadcrumb trail**: Small, warm gray, shows location: `Oefeningen / URI Builder`

### Exercise Interactions
- **Correct answer**: Sage green left-border slides in + explanation fades up. No bouncing checkmarks.
- **Incorrect answer**: Gentle rust-toned highlight + hint text appears below in italic. Encouraging, never punishing.
- **Score display**: Typographic — large Fraunces number (e.g., "4/6") with small DM Sans label below. No progress bars.
- **Transitions**: `ease-out-quart` (cubic-bezier(0.25, 1, 0.5, 1)), 300ms. Smooth deceleration only.

### Code & URI Display
```
┌──────────────────────────────────────┐
│  ▎ GET  /games?genre=rpg             │  ← dark bg, warm charcoal
│                                      │     method in ochre, path in cream
│                                      │     generous padding
└──────────────────────────────────────┘
```
- HTTP methods colored in `--accent-ochre`
- Paths in cream/white on dark background
- Query params subtly distinguished from base path

### Responsive
- Desktop: Magazine-width content column (~720px) with margin space for pull quotes
- Tablet: Full-width content, pull quotes inline
- Mobile: Stacked layout, touch-friendly drag targets (min 48px), exercises adapt (not shrink)

### Page-Specific Design Notes

**Homepage**: Large Fraunces headline ("Leer REST APIs begrijpen"), editorial subtitle, then a staggered grid of exercise links — not uniform cards, but varied sizes with the most important exercises larger.

**URI Builder**: Split layout — scenario description (left, body text) and URI input field (right, code-styled). Input has a monospace font, dark background, feels like typing in a terminal.

**URI Spotter**: Two URI specimens side by side on dark blocks. Student clicks one. The chosen one either glows sage (correct) or gently pulses rust (incorrect).

**REST Quiz**: One question at a time, full-width. Scenario in large italic body text. Options as text buttons with generous spacing (not radio buttons).

**JSON Explorer**: Dark code panel (full width) with collapsible tree. Challenge question floats above in a warm cream banner. Clicking a JSON value highlights it in ochre.

**Architecture Builder**: Vertical stack area with dashed drop zones. Draggable layer cards with subtle shadow on pickup. Connected layers show a thin sage-green line between them.

**Glossary**: Alphabetical sections with large Fraunces letter headers (like a dictionary). Terms as definition lists, not cards.

## Verification

- Run `npm run dev` and test each page
- Test all exercises manually (correct + incorrect answers)
- Test JSON Explorer with live API fetch
- Test drag-and-drop on mobile/touch
- Run `npm run build` and verify static output
- Deploy to GitHub Pages or Cloudflare and verify
