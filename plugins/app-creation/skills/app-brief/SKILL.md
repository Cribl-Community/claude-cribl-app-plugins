---
name: app-brief
description: Generates the app brief from the app definition, or updates it if APP_BRIEF.md already exists
when_to_use: After completing APP_DEFINITION.md, to generate a comprehensive implementation brief. Can also be run again to update an existing brief after changes to APP_DEFINITION.md
---

<command-name>
app-brief
</command-name>

<command-description>
Generate or update the app brief based on APP_DEFINITION.md. On first run, creates APP_BRIEF.md from scratch. On subsequent runs, detects changes to APP_DEFINITION.md and updates the existing brief incrementally.
</command-description>

<command-examples>
/app-brief
</command-examples>

## Pre-flight Check

Before proceeding, verify you are in a scaffolded Cribl app folder. If you are not, stop and tell the user: "You must run this skill from within a scaffolded Cribl app folder."

**Trigger:** Run this skill in the same folder where the `APP_DEFINITION.md` resides.

**Input:** Contents of `APP_DEFINITION.md` (and optionally existing `APP_BRIEF.md` if updating)

**Output:** A comprehensive app brief (with full editing capability) that Claude Code can use to implement the app which is stored in `APP_BRIEF.md`

## How to Use

1. `cd` into the app directory
2. Run this skill: `/app-brief`
   - **First run**: Creates `APP_BRIEF.md` from the definition
   - **Subsequent runs**: Updates `APP_BRIEF.md` based on changes to `APP_DEFINITION.md`
3. Review and edit the generated brief
4. Run the `/app-validate` skill
5. If validation fails, fix the brief.
6. Pass the contents of `app-brief` to your agentic tool (Claude Code)

## Skill Workflow

### Step 1: Check for Existing Brief

- If `APP_BRIEF.md` does not exist: Run the "Generate the Brief" workflow (see below)
- If `APP_BRIEF.md` already exists: Run the "Update the Brief" workflow (see below)

### Step 2A: Generate the Brief (First Run)

If `APP_BRIEF.md` does not exist, generate the implementation brief from scratch based on answers provided in `APP_DEFINITION.md`

```markdown
# [App Name] - App Brief

## App ID
[The unique identifier for this app]

## Problem & Vision
[Summary of the problem the app solves and how users benefit]

## Target Users
[Who uses this app and what they're trying to accomplish]

## Key Workflows
### Workflow 1: [Name]
- **User sees**: [What appears on screen]
- **User does**: [What they click/configure/select]
- **Result**: [What gets created/changed/shown]
- **Permissions**: [Who can do this? Any restrictions?]

### Workflow 2: [Name]
- **User sees**: [What appears on screen]
- **User does**: [What they click/configure/select]
- **Result**: [What gets created/changed/shown]
- **Permissions**: [Who can do this? Any restrictions?]

[+ Additional workflows as needed]

## Data & Actions
**The app will fetch from Cribl:**
- [Resource type 1]: [Why? Used in which workflow?]
- [Resource type 2]: [Why? Used in which workflow?]

**The app will create/modify/delete in Cribl:**
- [Resource type 1]: [Which workflow creates/modifies/deletes this?]
- [Resource type 2]: [Which workflow creates/modifies/deletes this?]

**The app will remember (general state):**
- [State 1]: [What should be saved? When should it persist?]
- [State 2]: [What should be saved? When should it persist?]

(Or "None" if no general state)

**User-specific settings (stored per user):**
- [Setting 1]: [What is it? Which user(s) need it?]
- [Setting 2]: [What is it? Which user(s) need it?]

(Or "None" if no per-user settings)

**Secure secrets:**
- [Secret 1]: [What is it? How is it used?]
- [Secret 2]: [What is it? How is it used?]

(Or "None" if no secrets)

## UI Structure
### Overall Layout
[Recommended structure: wizard, dashboard, form, table, etc.]

### Key Screens/Pages
1. **[Screen Name]**: [What it shows, what user can interact with]
2. **[Screen Name]**: [What it shows, what user can interact with]
3. **[Screen Name]**: [What it shows, what user can interact with]

[Add more screens as needed]

## Permissions & Access
- **Who can use this app?**: [All members, or specific roles?]
- **Permission-aware behavior**: [How does the app respond if a user lacks access to an action?]

## External Integrations (if any)
- **[Service name]**: [What does the app do with it?]
- **[Service name]**: [What does the app do with it?]

(Or "None" if the app only works with Cribl)

## MVP Scope
**Must-have:**
- [Feature 1]
- [Feature 2]

**Nice-to-have (defer):**
- [Feature 1]
- [Feature 2]

**Out of scope:**
- [Feature 1]

## Edge Cases & Error Handling
- **If user lacks permission**: [What should the app show?]
- **If data is unavailable**: [What should the app show?]
- **If an action fails**: [What should the app show?]

## Implementation guidance (include this section verbatim)
- Read AGENTS.md first
- Then read openapi.json
- NEVER EVER use local storage 

---
```

### Step 2B: Update the Brief (Subsequent Runs)

If `APP_BRIEF.md` already exists, intelligently update it based on changes to `APP_DEFINITION.md`:

**Detection & Merging Logic:**
1. Compare the two files to identify what changed in `APP_DEFINITION.md`:
   - App ID changed
   - New workflows added
   - Workflows modified (name changed, description changed, or steps changed)
   - New data requirements (new fetch sources, create/modify/delete operations, state fields)
   - New UI screens or layout changes
   - New scope items (must-have, nice-to-have, out-of-scope)
   - Changes to permissions, integrations, or edge cases

2. **For new workflows**: Add them to the "## Key Workflows" section in `APP_BRIEF.md`, following the same format as existing workflows

3. **For modified workflows**: Update the corresponding workflow section with the new details, preserving any elaborations or examples that were added to the brief but not in the definition

4. **For new data requirements**: Add them to the "## Data & Actions" section under the appropriate subsection (fetch, create/modify/delete, state, settings, secrets)

5. **For new UI screens**: Add them to the "## UI Structure" section's "Key Screens/Pages" list

6. **For scope changes**: Add new items to "## MVP Scope" under the appropriate subsection

7. **For other changes**: Update the relevant sections (Problem & Vision, Target Users, Permissions & Access, External Integrations, Edge Cases & Error Handling)

**Preservation Rules:**
- Never remove sections or workflows that exist in `APP_BRIEF.md` but are not mentioned in the updated `APP_DEFINITION.md`
- Keep any elaborations, examples, or implementation details that were added to the brief beyond what was in the definition
- If a workflow description in the definition is more detailed, replace the brief's version; if the brief is more detailed, keep it
- Always preserve the "## Implementation guidance" section exactly as is

**Output:**
- Display a summary of what changed: "Added 1 new workflow: [name]. Updated 2 workflows: [names]. Added 3 data requirements."
- Save the updated brief back to `APP_BRIEF.md`
- Show the user the updated sections for review

## Workflow Exit

Once the brief is generated or updated, the final brief is ready for:
- **First run**: Review, edit if needed, then run `/app-validate` to validate alignment
- **Subsequent runs**: Review the delta summary, check the updated sections, then commit changes via `/app-implement`

---

## Key Principles

- **No technical jargon**: Users describe problems; Claude figures out implementation
- **Markdown persistence**: Easy to read, edit, and version control
- **Iterative updates**: Brief can be regenerated/updated as requirements evolve
- **Incremental updates**: Only changed sections are rewritten; existing elaborations are preserved
- **Direct file editing**: Users can edit `APP_BRIEF.md` directly for fast iteration
- **APIs handled by Claude**: Claude uses `openapi.json` to determine which Cribl APIs to call
- **User-specific storage**: Per-user settings automatically prefixed with user ID
- **MVP-first**: Encourage shipping the minimum and iterating
- **External integrations only**: List third-party services if the app calls them
- **Single source of truth**: `APP_DEFINITION.md` is the authoritative source; changes there drive brief updates

## Example Workflow: Adding a New Feature

1. Edit `APP_DEFINITION.md` to add a new workflow (e.g., "Bulk enable/disable integrations")
2. Run `/app-brief` — the skill detects the new workflow and adds it to the brief
3. Review the updated brief to see the new workflow integrated with existing ones
4. Run `/app-validate` to ensure the updated brief is sound
5. Run `/app-implement` to implement the new workflow

## Resources

- **Cribl Apps Docs**: https://docs.cribl.io/apps/
- **Builder Guide**: https://docs.cribl.io/apps/builder-guide/
- **GitHub Examples**: https://github.com/criblapps and https://github.com/Cribl-Community

