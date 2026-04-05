---
name: github-skill-finder
description: Searches GitHub for relevant Claude skills whenever a user asks to find, search for, or install a skill, mentions a task that might benefit from a skill, or says anything like "find me a skill for X", "is there a skill that can help with Y", "look for a skill on GitHub", or "any skills available for Z". Also proactively suggest searching GitHub when the user describes a complex or specialized task and no locally installed skill covers it. Use this skill any time it seems like a GitHub skill might help — even if the user doesn't explicitly mention GitHub or skills.
---

# GitHub Skill Finder

Helps users discover and install Claude skills from GitHub.

## Workflow

### Step 1: Understand What the User Needs

If the user hasn't already described what they want to do, ask in one sentence. Extract the **core task or topic** to use as search terms (e.g., "pdf generation", "data visualization", "email drafting").

### Step 2: Search GitHub for Skills

Use web search to find relevant skills on GitHub. Run 2–3 searches with different phrasings:

```
Search queries to try:
1. github Claude skill "<topic>" SKILL.md
2. github "claude skill" "<topic>" filetype:md
3. github "<topic>" skill claude anthropic SKILL.md
```

Look for repositories that contain a `SKILL.md` file — that's the telltale sign of a Claude skill. Also look for `.skill` packaged files.

### Step 3: Fetch and Summarize Each Candidate

For each promising result (up to 5):
- First, try fetching the repo's main GitHub page (e.g. `https://github.com/org/repo`) — this is more reliably accessible than raw URLs
- If that's blocked, rely on the README content and search result snippets — these usually contain enough info
- **Do NOT attempt `raw.githubusercontent.com` URLs directly** — these are often blocked by network restrictions
- Extract: skill name, what it does, when it triggers, any dependencies
- Note the repo URL, stars, and last updated date if visible

**Fallback strategy if fetching fails:** Use search snippet content to summarize the skill. A good README and search preview almost always captures the essential "what it does" and "when to use it" info needed for the user to decide.

### Step 4: Present Options to the User

Show a clear comparison like this:

```
Found X skills that might help:

1. **skill-name** (github.com/org/repo ⭐123)
   Does: [one-line summary]
   Best for: [use cases]
   Last updated: [date]

2. **another-skill** ...

Which one would you like to install? (or "none of these" to search differently)
```

### Step 5: Install the Chosen Skill

Once the user picks one:

1. Try fetching the skill's main GitHub page to get full SKILL.md content
2. If the network blocks direct GitHub fetches, reconstruct the SKILL.md from:
   - The README (often contains the full skill description)
   - Search result snippets
   - The author's blog/docs if linked
3. Write the SKILL.md (and any reference files mentioned) to `/mnt/skills/user/<skill-name>/`
4. Confirm installation: "✅ Installed! The **skill-name** skill is now active."

**Note:** `curl` and `raw.githubusercontent.com` URLs may be blocked in the container. If so, use web_fetch on the main GitHub repo page, or reconstruct the skill from search content — this works well in practice since READMEs and search previews capture the key instructions.

If copying to `/mnt/skills/user/` fails due to permissions, inform the user and suggest they copy the files manually or contact their admin.

### Step 6: Offer a Quick Test

After installing, say: "Want me to try it out right now? Just describe a task and I'll use the new skill."

---

## Search Tips

- If broad GitHub search returns too many irrelevant results, narrow by adding "claude skill anthropic SKILL.md" to the query
- If no results found, try synonyms (e.g., "spreadsheet" instead of "excel")
- Skills are often in repos named `claude-skills`, `claude-skill-*`, or under orgs like `anthropics-skills` — check these if broad search is weak
- Always prefer skills with recent commits and clear SKILL.md descriptions

## Edge Cases

- **No skills found**: Tell the user honestly, and offer to help them create one using the skill-creator skill
- **Multiple great matches**: Show all of them and let the user decide — don't pick for them
- **Skill has dependencies** (e.g., requires Python packages): Call these out clearly before installing
- **Private repos**: If a repo is private or requires auth, explain you can't access it and suggest the user clone it manually
