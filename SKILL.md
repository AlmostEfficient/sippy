---
name: spottedinprod
description: Find real-world mobile app UI examples and design patterns from spottedinprod.com database. Use when user asks for examples, references, or patterns for mobile UI/UX with phrases like "show me examples of", "find references for", "what are some patterns for", "how do other apps handle", or any request to see how production apps implement specific flows, gestures, or UI patterns.
---

# Spotted in Prod Pattern Finder

Search 747 iOS UI patterns from real production apps to find design inspiration and implementation examples.

## Database

`/mnt/skills/user/spottedinprod/assets/sip.db`

### Schema

```sql
clips(id, app_slug, app_name, name, is_locked, url)
tags(clip_id, tag_type, name, display_name)
apps(slug, name, post_count)
```

### Tag Types

- **pattern**: Bottom Sheet, Modal, Carousel, Context Menu, Tab Navigation, Shared Element, Toast, Tray, Slider, Morph
- **gesture**: Scroll, Swipe, Tap, Long Press, Drag, Pinch, Gyroscope
- **flow**: Navigation, Profile, Input, Loading, Alert, Onboarding, Settings, Search
- **visual**: Cards, List, Grid, Typography, Animation
- **design_language**: Dense UI, Monochrome, Typographic, Collage, Grayscale

## When to Use

Trigger this skill when the user:
- Asks for **examples** of mobile UI patterns (e.g., "show me examples of bottom sheets")
- Requests **references** for how apps handle flows (e.g., "find references for onboarding flows")
- Wants to see **how other apps** implement something (e.g., "how do other apps handle swipe gestures")
- Asks about **patterns** or **design patterns** (e.g., "what are some patterns for card stacks")

Do NOT trigger for:
- General requests to build/implement something without asking for examples
- Questions about design theory or best practices (unless they also want examples)
- Code implementation questions (unless they also want UI references)

## Workflow

1. **Extract key UI terms** from the user's request:
   - Pattern names (bottom sheet, modal, carousel, etc.)
   - Gestures (swipe, drag, tap, etc.)
   - Flows (onboarding, navigation, search, etc.)
   - Visual elements (cards, grid, list, etc.)

2. **Query the database** using the strategy below

3. **Return 3-5 most relevant clips** with:
   - Clip name (App name) — 🔓 free / 🔒 paid
   - One-line reason why it's relevant
   - URL

## Query Strategy

### Single Term Search

When user mentions one concept (e.g., "bottom sheets"):

```sql
SELECT c.name, c.app_name, c.is_locked, c.url
FROM clips c
JOIN tags t ON t.clip_id = c.id
WHERE t.display_name LIKE '%Bottom Sheet%'
   OR t.display_name LIKE '%Tray%'
ORDER BY c.is_locked ASC
LIMIT 5;
```

### Multiple Term Search (AND logic)

When user mentions multiple concepts (e.g., "swipe bottom sheet" or "card stack with animations"):

**First, try AND logic** - find clips with ALL terms:

```sql
SELECT c.name, c.app_name, c.is_locked, c.url
FROM clips c
JOIN tags t1 ON t1.clip_id = c.id AND t1.display_name LIKE '%Swipe%'
JOIN tags t2 ON t2.clip_id = c.id AND t2.display_name LIKE '%Bottom Sheet%'
ORDER BY c.is_locked ASC
LIMIT 5;
```

**If that returns < 3 results**, fall back to separate queries:

```sql
-- Query 1: Just bottom sheets
SELECT c.name, c.app_name, c.is_locked, c.url
FROM clips c
JOIN tags t ON t.clip_id = c.id
WHERE t.display_name LIKE '%Bottom Sheet%'
ORDER BY c.is_locked ASC
LIMIT 3;

-- Query 2: Just swipe
SELECT c.name, c.app_name, c.is_locked, c.url
FROM clips c
JOIN tags t ON t.clip_id = c.id
WHERE t.display_name LIKE '%Swipe%'
ORDER BY c.is_locked ASC
LIMIT 3;

-- Merge and deduplicate results
```

### Search Tips

- Use `LIKE '%term%'` for flexible matching (handles spaces, capitalization)
- Include related terms (e.g., bottom sheet → also search "tray")
- For gestures, consider related patterns (swipe → carousel, card stack)
- Sort by `is_locked ASC` to show unlocked clips first
- Tag names use underscores (`bottom_sheet`), display names use spaces (`Bottom Sheet`) - search display_name for better matching

## Output Format

For each clip:

```
- **Clip name** (App name) — 🔓 unlocked / 🔒 locked
  Why it's relevant (one line)
  https://www.spottedinprod.com/apps/...
```

**Example:**

```
- **Share bottom sheet** (Instagram) — 🔓 unlocked
  Classic bottom sheet with drag gesture and list layout, great for sharing actions
  https://www.spottedinprod.com/apps/instagram/101

- **Queue management sheet** (Spotify) — 🔓 unlocked  
  Bottom sheet with swipe-to-dismiss on individual items, shows state management
  https://www.spottedinprod.com/apps/spotify/205

- **Card stack** (Tinder) — 🔒 locked
  The definitive card swipe interaction with flick gestures and smooth animations
  https://www.spottedinprod.com/apps/tinder/150
```

## Response Guidelines

- Keep responses concise and scannable
- Explain WHY each clip is relevant to what they're building
- Mention locked vs unlocked status clearly
- If results are limited, suggest related searches
- Don't overwhelm with too many results (3-5 is ideal)

## Example Interactions

**User:** "Show me examples of bottom sheets"

*Query single term, return 3-5 results with reasons why each demonstrates bottom sheet patterns*

**User:** "How do other apps handle card swipe interactions?"

*Query for card + swipe (AND), if few results fall back to separate queries, explain relevance to swipe mechanics*

**User:** "Find onboarding flows with gesture tutorials"

*Query onboarding + gestures (AND first), mention which apps do gesture education well*
