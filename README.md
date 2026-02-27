# Spotted in Prod - Mobile UI Reference Skill

Search 747+ real production app UI patterns for design inspiration.

## Installation

1. Copy this `spottedinprod/` directory to your Claude skills folder
2. Replace `assets/sip.db` with your actual database
3. Restart Claude Code (if needed)

## Usage

Ask Claude for examples using trigger phrases:
- "Show me examples of bottom sheets"
- "Find references for card swipe interactions"
- "How do other apps handle onboarding?"
- "What are some patterns for modals?"

Claude will search the database and return 3-5 relevant clips with explanations.

## Database

Expects SQLite database at `assets/sip.db` with schema:
```sql
clips(id, app_slug, app_name, name, is_locked, url)
tags(clip_id, tag_type, name, display_name)
apps(slug, name, post_count)
```

Sample database included for testing.
