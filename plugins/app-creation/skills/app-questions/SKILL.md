---
name: app-questions
description: Guides defining the problem the app solves, and associated workflows. Also provides an interactive editor for updating existing definitions
when_to_use: At the start of a new Cribl app project to define the problem and workflows. Can also be run again to edit/update an existing APP_DEFINITION.md
---

<command-name>
app-questions
</command-name>

<command-description>
Generate a new APP_DEFINITION.md or edit an existing one interactively. On first run, guides you through defining the problem and workflows. On subsequent runs, shows a menu to view/edit/add/delete parts of the definition.
</command-description>

<command-examples>
/app-questions
</command-examples>

## Pre-flight Check

Before proceeding, verify you are in a scaffolded Cribl app folder. If you are not, stop and tell the user: "You must run this skill from within a scaffolded Cribl app folder."

**Trigger:** Run this skill inside a Cribl app project directory.

**Input:** 
- **First run**: Answer questions about the app's purpose, workflows, and problems it solves
- **Subsequent runs**: Choose menu options to view/edit/add/delete definition sections

**Output:** 
- **First run**: An app definition capturing all answers (`APP_DEFINITION.md`)
- **Subsequent runs**: Updated `APP_DEFINITION.md` with your changes

## How to Use

### First Run (Creation Mode)

1. Scaffold an app in Cribl (Apps > Create App)
2. Copy the starter code to your local machine
3. `cd` into the app directory
4. Run this skill: `/app-questions`
5. **Describe what your app does** (be specific about the problem and who uses it)
6. Answer clarifying questions if needed to sharpen the description
7. Answer the full questionnaire about your app
8. Review and edit the generated app definition

**💡 Tip:** Provide specifics in your initial description. Instead of "monitoring tool", say "help our platform team track when customer data pipelines fall behind their SLAs".

### Subsequent Runs (Edit Mode)

1. Run `/app-questions` (the skill detects `APP_DEFINITION.md` exists)
2. A menu appears with options to edit parts of the definition
3. Choose what to update:
   - View/edit the problem statement
   - Manage workflows (add/edit/delete)
   - Update data & integration points
   - Modify permissions & access rules
   - Change scope items
   - Update UI preferences
4. Make your changes interactively
5. Review the updated definition
6. Exit when done

**Workflow Example:** Add a new workflow by running `/app-questions`, selecting "Manage Workflows", choosing "Add", and describing the new workflow. Then run `/app-brief` to update the app brief, and `/app-implement` to implement it.

## Skill Workflow

### Step 1: Detect Mode

When the skill runs:
- **If `APP_DEFINITION.md` does not exist**: Enter "Creation Mode" (see below)
- **If `APP_DEFINITION.md` exists**: Enter "Edit Mode" (see below)

### Creation Mode: Gather information through 7 phases

The skill gathers information through 7 phases, with **clarity validation at every step** to ensure specific, thorough answers:

**Clarity validation triggers on:**
- Very short or one-word answers (< 2 sentences)
- Missing key context or vague language
- Abstract statements without concrete details
- Answers that don't address the question

When clarity issues are detected, the skill asks targeted follow-up questions to sharpen the answer before proceeding to the next phase.

### Phase 1: Understand the Problem
**Guided discovery — ask ONE question at a time:**
1. Ask: "What's the main problem this app solves?"
   - Wait for answer, clarify if needed
2. Ask: "Who will use this app?"
   - Wait for answer, clarify if needed
3. Ask: "How are they currently solving this problem?"
   - Wait for answer, clarify if needed

**Clarity check:** Answers must include the problem, user role, and current workflow. Follow up if vague or missing context. Do NOT proceed to the next question until the current answer is clear and complete.

### Phase 2: Map User Workflows & Key Tasks
**Guided workflow discovery:**
- Ask for **Workflow 1** with a description of what the user does step-by-step
- For each workflow, clarify:
  - What specific steps does the user take (in order)?
  - What do they create, modify, or delete?
  - What do they see or review?
  - Any decision points or branches?
- Once clear, move to **Workflow 2**, then 3, etc.
- Allow the user to enter blank or "done" to finish workflow collection

**Clarity check:** Ensure each workflow is concrete, sequenced, and includes specific objects/data and actions. Don't proceed to the next workflow until the current one is clear.

### Phase 3: Explore Data & Integration Points
**Guided discovery — ask ONE question at a time:**
1. Ask: "What data does the app display?"
   - Wait for answer, clarify if needed
2. Ask: "What does the app create or change?"
   - Wait for answer, clarify if needed
3. Ask: "Does the app call any external services (Slack, OpenAI, etc.)?"
   - Wait for answer, clarify if needed

**Clarity check:** Answers must specify data sources and integration names. Do NOT proceed to the next question until the current answer is clear and complete.

### Phase 4: Consider Permissions & Access
**Guided discovery — ask ONE question at a time:**
1. Ask: "Should different users see different data?"
   - Wait for answer, clarify if needed
2. Ask: "How should the app behave if a user lacks permission?"
   - Wait for answer, clarify if needed

**Clarity check:** Answers must define user roles and access rules. Do NOT proceed to the next question until the current answer is clear and complete.

### Phase 5: Ask About State & Secrets
**Guided discovery — ask ONE question at a time:**
1. Ask: "Does the app need to save any state?"
   - Wait for answer, clarify if needed
2. Ask: "Are there general settings?"
   - Wait for answer, clarify if needed
3. Ask: "Are there any user-specific settings?"
   - Wait for answer, clarify if needed
4. Ask: "Does the app handle any secure secrets?"
   - Wait for answer, clarify if needed

**Clarity check:** Answers must specify what state is saved and where. Do NOT proceed to the next question until the current answer is clear and complete.

### Phase 6: Narrow the Scope
**Guided discovery — ask ONE question at a time:**
1. Ask: "What's the absolute minimum (MVP)?"
   - Wait for answer, clarify if needed
2. Ask: "What would be nice to add later?"
   - Wait for answer, clarify if needed
3. Ask: "Anything definitely out of scope?"
   - Wait for answer, clarify if needed

**Clarity check:** MVP must be realistic and deliverable. Do NOT proceed to the next question until the current answer is clear and complete.

### Phase 7: Clarify Preferences
**Guided discovery — ask ONE question at a time:**
1. Ask: "What's the overall structure? (wizard, dashboard, form, table, etc.)"
   - Wait for answer, clarify if needed
2. Ask: "Any specific look/feel preferences?"
   - Wait for answer, clarify if needed

**Clarity check:** Answers must align with the workflows and tasks identified. Do NOT proceed to the next question until the current answer is clear and complete.

---

## Re-entrancy: Resume Where You Left Off

The skill is **fully re-entrant**. When you run `/app-questions`:
1. If `APP_DEFINITION.md` doesn't exist, the skill starts from Phase 1
2. If `APP_DEFINITION.md` exists, the skill reads it and resumes from the first incomplete phase
3. The skill skips questions for phases that are already answered and complete
4. **Each answer is written incrementally** to `APP_DEFINITION.md` as you complete questions, so no data is lost if you exit early

This means you can run the skill incrementally: define the problem today, map workflows tomorrow, finish scope and preferences next week. All your answers are preserved.

## Persistence: APP_DEFINITION.md

The skill persists answers incrementally to `APP_DEFINITION.md` in the app directory as you progress through each question. This file serves as both:
- **Input:** When you resume, the skill reads this file to determine which phase to continue from
- **Output:** Your complete app definition once all phases are done

```markdown
# App Definition

## Problem
[User's answer about what problem this solves]

## Target Users
[Who uses this and why]

## Workflows
### Workflow 1: [Name]
[Full description]

### Workflow 2: [Name]
[Full description]

[+ Additional workflows]

## Data & Integration Points
### Data Display
[User's answer]

### Create/Modify/Delete
[User's answer]

### External Integrations
[User's answer or "None"]

## Permissions & Access
### Different Users See Different Data?
[User's answer]

### Permission-Denied Behavior
[User's answer]

## State & Secrets
### Saved State
[User's answer or "None"]

### General-settings
[User's answer or "None"]

### User-Specific Settings
[User's answer or "None"]

### Secure Secrets
[User's answer or "None"]

## Scope
### Must-Have (MVP)
[User's answers]

### Nice-to-Have (Defer)
[User's answers]

### Out of Scope
[User's answers]

## UI Preferences
### Overall Structure
[User's answer]

### Look/Feel
[User's answer]
```

**Users can edit this file directly** in their editor, or re-run the skill to update via Q&A.

---

## Edit Mode: Interactive Definition Editor

When `APP_DEFINITION.md` already exists, the skill enters **Edit Mode** and presents an interactive menu:

```
=== APP_DEFINITION.md Editor ===

1. View/Edit Problem Statement
2. View/Edit Target Users
3. Manage Workflows (add, edit, delete, list)
4. View/Edit Data & Integration Points
5. View/Edit Permissions & Access
6. View/Edit Scope (must-have, nice-to-have, out-of-scope)
7. View/Edit UI Preferences
8. View entire definition (read-only)
9. Exit and save

Enter choice (1-9):
```

### Menu Option Workflows

#### Option 1: View/Edit Problem Statement
- Display current problem statement
- Ask: "Edit this? (yes/no)"
- If yes: Present current text and ask for new text
- Update `APP_DEFINITION.md` and return to menu

#### Option 2: View/Edit Target Users
- Display current target users
- Ask: "Edit this? (yes/no)"
- If yes: Present current text and ask for new text
- Update `APP_DEFINITION.md` and return to menu

#### Option 3: Manage Workflows
Show sub-menu:
```
=== Manage Workflows ===

1. List all workflows
2. Add a new workflow
3. Edit a workflow
4. Delete a workflow
5. Back to main menu

Enter choice (1-5):
```

**Sub-option 3.1: List all workflows**
- Display all workflow names and descriptions
- Return to sub-menu

**Sub-option 3.2: Add a new workflow**
- Ask: "Workflow name?"
- Ask: "Workflow description?" (can be multi-line)
- Add to `## Workflows` section following existing format
- Update `APP_DEFINITION.md` and return to sub-menu

**Sub-option 3.3: Edit a workflow**
- List all workflows with numbers
- Ask: "Which workflow to edit? (1-N)"
- Display current workflow text
- Ask: "New description?" (can be multi-line)
- Update `APP_DEFINITION.md` and return to sub-menu

**Sub-option 3.4: Delete a workflow**
- List all workflows with numbers
- Ask: "Which workflow to delete? (1-N)"
- Ask: "Are you sure? (yes/no)"
- Remove from `APP_DEFINITION.md` and return to sub-menu

#### Option 4: View/Edit Data & Integration Points
- Display current sections:
  - Data Display
  - Create/Modify/Delete
  - External Integrations
- Show sub-menu:
  ```
  1. Edit Data Display
  2. Edit Create/Modify/Delete
  3. Edit External Integrations
  4. Back to main menu
  ```
- For each: Display current, ask for edits, update file

#### Option 5: View/Edit Permissions & Access
- Display current sections:
  - Different Users See Different Data?
  - Permission-Denied Behavior
- Ask which to edit, apply updates
- Update `APP_DEFINITION.md` and return to menu

#### Option 6: View/Edit Scope
- Display current sections:
  - Must-Have (MVP)
  - Nice-to-Have (Defer)
  - Out of Scope
- Show sub-menu for each section:
  - View all items
  - Add item
  - Delete item
- Update `APP_DEFINITION.md` and return to menu

#### Option 7: View/Edit UI Preferences
- Display current sections:
  - Overall Structure
  - Look/Feel
- Ask which to edit, apply updates
- Update `APP_DEFINITION.md` and return to menu

#### Option 8: View entire definition (read-only)
- Display full `APP_DEFINITION.md` content
- Return to menu

#### Option 9: Exit and save
- Show summary of what changed this session
- Confirm save (changes are already written incrementally)
- Exit

### Edit Mode UX Principles
- **Incremental saves**: Write to `APP_DEFINITION.md` after each change
- **Confirmations for destructive actions**: Always ask before deleting workflows or scope items
- **Show current state**: Always display current content before asking for edits
- **Easy back out**: Always allow returning to previous menu without committing
- **Multi-line input**: Support multi-line text for workflow descriptions, data requirements, etc.
- **Summary on exit**: Show what was modified when exiting

---

## Resources

- **Cribl Apps Docs**: https://docs.cribl.io/apps/
- **Builder Guide**: https://docs.cribl.io/apps/builder-guide/
- **GitHub Examples**: https://github.com/criblapps and https://github.com/Cribl-Community

