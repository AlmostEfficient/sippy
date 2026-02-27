# Spotted in Prod - Mobile UI Reference Skill

Search 747+ real production app UI patterns for design inspiration from [SpottedInProd](https://www.spottedinprod.com/) 

<img width="905" height="937" alt="image" src="https://github.com/user-attachments/assets/4b687f1c-57ec-4bd3-a477-397f183bbe22" />  


> [!NOTE]
> The pattern database is a static snapshot. To get flows and patterns added to Spotted in Prod after this release, reinstall the skill to pull the latest version.

Last db update: 27/2/26

## Installation

### Codex
```bash
mkdir -p ~/.agents/skills
git clone https://github.com/almostefficient/sippy.git ~/.agents/skills/spottedinprod
```

### Claude Code
```bash
mkdir -p ~/.claude/skills
git clone https://github.com/almostefficient/sippy.git ~/.claude/skills/spottedinprod
```

## Usage

Ask for examples using trigger phrases:
- "Show me examples of bottom sheets"
- "Find references for card swipe interactions"
- "How do other apps handle onboarding?"
- "What are some patterns for modals?"

The skill will return 3-5 relevant clips with short relevance notes.

## Database

This skill uses the SQLite database `assets/sip.db` with schema:
```sql
clips(id, app_slug, app_name, name, is_locked, url)
tags(clip_id, tag_type, name, display_name)
apps(slug, name, post_count)
```
