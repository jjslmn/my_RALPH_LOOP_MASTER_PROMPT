# RALPH Loop Enhanced Master Prompt v2

You are initializing a RALPH Loop project structure. The RALPH Loop is an autonomous AI development methodology that breaks projects into small, verifiable tasks and executes them one at a time across fresh sessions, maintaining context through persistent files.

**This enhanced version supports both NEW projects and FEATURE ADDITIONS to existing codebases.**

---

## 1. Create Directory Structure

```
./ralph/
├── IDEA.md
├── FEATURE_REQUEST.md      # NEW: For adding features to existing projects
├── DISCOVER.md             # NEW: Codebase analysis instructions
├── PRD.md  
├── prd.json
├── progress.txt
├── AGENTS.md
├── PROMPT.md
├── GENERATE_PRD.md
├── start.sh
├── archive/
│   └── completed/          # NEW: Archive for completed feature batches
└── templates/
    ├── IDEA_TEMPLATE.md
    ├── PRD_TEMPLATE.md
    └── FEATURE_TEMPLATE.md # NEW: Template for feature requests
```

---

## 2. Create File: ralph/IDEA.md

```markdown
# PROJECT IDEA

<!-- 
Choose your mode:
- NEW PROJECT: Fill out "My Idea" section
- EXISTING PROJECT: Fill out "Existing Project Context" AND "New Features"
-->

## Mode: [NEW / EXISTING]

---

## My Idea (For NEW projects)

[Write your idea here]

---

## Existing Project Context (For EXISTING projects)

### Project Location
- **Root directory:** [e.g., /path/to/myproject]
- **Main language(s):** [e.g., TypeScript, Python]
- **Framework(s):** [e.g., Next.js, FastAPI]

### What Already Exists
<!-- Brief description of current functionality -->
[Describe what the project currently does]

### Current Architecture
<!-- High-level overview -->
- **Frontend:** [if applicable]
- **Backend:** [if applicable]  
- **Database:** [if applicable]
- **Key directories:** [e.g., src/, api/, components/]

### Existing Patterns to Preserve
<!-- Important conventions Claude should follow -->
- Naming conventions:
- File organization:
- Testing approach:
- Other patterns:

---

## New Features / Enhancements Wanted

<!-- What do you want to ADD to the project? -->
1. [Feature 1]
2. [Feature 2]
3. [Feature 3]

---

## Technical Context

- **Language/Framework preferences:** 
- **Target platform(s):** 
- **Existing systems to integrate:** 
- **Constraints or requirements:** 

## Additional Notes

[Any other context]
```

---

## 3. Create File: ralph/FEATURE_REQUEST.md (NEW)

```markdown
# FEATURE REQUEST

Use this file when adding NEW features to an existing RALPH-managed project.

## Feature Batch: [Name/Version, e.g., "v2.0 - User Dashboard"]

### Requested Features

#### Feature 1: [Name]
**Description:** [What it should do]
**User Story:** As a [user], I want [feature] so that [benefit]
**Acceptance Criteria:**
- [ ] Criterion 1
- [ ] Criterion 2

#### Feature 2: [Name]
**Description:** [What it should do]
**User Story:** As a [user], I want [feature] so that [benefit]
**Acceptance Criteria:**
- [ ] Criterion 1
- [ ] Criterion 2

### Technical Constraints
- Must integrate with: [existing systems]
- Cannot break: [existing functionality]
- Performance requirements: [if any]

### Priority Order
1. [Most important feature]
2. [Second priority]
3. [Third priority]

---

## Instructions for Claude

After filling this out, start a new Claude session and say:

"Read ralph/DISCOVER.md first to analyze the existing codebase, 
then read ralph/FEATURE_REQUEST.md and generate new user stories 
to append to prd.json"
```

---

## 4. Create File: ralph/DISCOVER.md (NEW - Critical for existing projects)

```markdown
# CODEBASE DISCOVERY INSTRUCTIONS

**Run this BEFORE generating or updating a PRD for an existing project.**

You are analyzing an existing codebase to understand its patterns, structure, and conventions before adding new features.

## Discovery Process

### Step 1: Understand Project Structure
```bash
# Get directory overview (adjust path as needed)
find . -type f -name "*.ts" -o -name "*.tsx" -o -name "*.py" -o -name "*.js" -o -name "*.jsx" | head -50
tree -L 3 -I 'node_modules|.git|__pycache__|.next|dist|build' || find . -type d | head -30
```

### Step 2: Identify Configuration & Conventions
```bash
# Check for config files that reveal patterns
cat package.json 2>/dev/null | jq '{scripts, dependencies, devDependencies}' || true
cat pyproject.toml 2>/dev/null || cat setup.py 2>/dev/null || true
cat tsconfig.json 2>/dev/null || true
cat .eslintrc* 2>/dev/null || true
cat .prettierrc* 2>/dev/null || true
```

### Step 3: Understand Existing Architecture
```bash
# Look at main entry points
cat src/index.* 2>/dev/null || cat app/main.* 2>/dev/null || cat main.* 2>/dev/null || true

# Check for existing patterns in key directories
ls -la src/ 2>/dev/null || ls -la app/ 2>/dev/null || true
```

### Step 4: Review Existing Tests
```bash
# Understand testing patterns
find . -name "*.test.*" -o -name "*.spec.*" -o -name "test_*" | head -10
cat $(find . -name "*.test.*" | head -1) 2>/dev/null | head -50 || true
```

### Step 5: Check Git History for Conventions
```bash
git log --oneline -20 2>/dev/null || true
```

## Output: Populate AGENTS.md

After discovery, UPDATE `ralph/AGENTS.md` with your findings:

```markdown
# AGENTS.md - Codebase Intelligence

## Project Overview
[What the project does - discovered from code]

## Tech Stack
- Language: [discovered]
- Framework: [discovered]
- Testing: [discovered]
- Build tools: [discovered]

## Discovered Patterns

### File Organization
[How files are structured]

### Naming Conventions
- Files: [e.g., kebab-case, PascalCase]
- Functions: [e.g., camelCase]
- Components: [e.g., PascalCase]

### Import Patterns
[How imports are organized]

### Testing Patterns
- Test location: [e.g., __tests__/, adjacent to source]
- Test naming: [e.g., *.test.ts, test_*.py]
- Testing framework: [e.g., Jest, pytest]

### Code Style
[Any notable patterns]

## Gotchas & Warnings
[Things to be careful about]

## Common Commands
- Run dev: [command]
- Run tests: [command]
- Build: [command]
- Lint: [command]

## Files NOT to Modify
[Any files that should be left alone]
```

## After Discovery

Once AGENTS.md is populated, proceed to:
1. If NEW project: `ralph/GENERATE_PRD.md`
2. If adding features: Generate new user stories from `ralph/FEATURE_REQUEST.md`
```

---

## 5. Create File: ralph/prd.json (Enhanced)

```json
{
  "project": "Project Name - Update after PRD generation",
  "version": "1.0.0",
  "mode": "new",
  "branchName": "ralph/main",
  "description": "Project description - Update after PRD generation",
  "existingProject": {
    "rootPath": "",
    "analyzedAt": "",
    "preservePatterns": []
  },
  "qualityGates": {
    "typecheck": "echo 'Configure typecheck command'",
    "lint": "echo 'Configure lint command'",
    "test": "echo 'Configure test command'",
    "existingTests": "echo 'Run existing project tests to ensure no regression'"
  },
  "featureBatches": [
    {
      "batchId": "initial",
      "batchName": "Initial Development",
      "createdAt": "",
      "completedAt": null,
      "userStories": []
    }
  ],
  "userStories": []
}
```

---

## 6. Create File: ralph/progress.txt (Enhanced)

```
================================================================================
RALPH LOOP PROGRESS LOG
================================================================================
Project: [Pending PRD generation]
Mode: [NEW / EXISTING]
Started: [Pending]
================================================================================

## EXISTING CODEBASE NOTES (if applicable)
<!-- 
For existing projects: key patterns discovered during DISCOVER phase.
New iterations MUST preserve these patterns.
-->

[Run DISCOVER.md to populate this section]

================================================================================

## CODEBASE PATTERNS
<!-- 
IMPORTANT: New iterations READ THIS SECTION FIRST.
Contains patterns that work, things to avoid, and accumulated wisdom.
-->

[No patterns yet - will be populated during iterations]

================================================================================

## REGRESSION NOTES
<!-- 
For existing projects: things that broke and how they were fixed.
CRITICAL for maintaining existing functionality.
-->

[No regressions logged yet]

================================================================================
## ITERATION LOG
================================================================================

[Awaiting first iteration]
```

---

## 7. Create File: ralph/AGENTS.md (Enhanced)

```markdown
# AGENTS.md - Codebase Intelligence

This file contains patterns, conventions, and learnings discovered during RALPH Loop iterations.
**Read this file at the start of each session.**

## Project Mode
- [ ] New Project
- [ ] Existing Project Enhancement

## Project Overview
[Populated after PRD generation or DISCOVER phase]

## Tech Stack
<!-- For existing projects, populated by DISCOVER.md -->
- Language:
- Framework:
- Testing:
- Build tools:

## EXISTING PATTERNS TO PRESERVE
<!-- 
CRITICAL for existing projects!
New code MUST follow these patterns.
-->

### File Organization
[How existing files are structured]

### Naming Conventions
[Existing naming patterns]

### Architecture Patterns
[e.g., MVC, component structure, API patterns]

## Discovered Patterns
<!-- What works well in this codebase -->

## Gotchas & Warnings  
<!-- Things to avoid or be careful about -->

## File Structure Notes
<!-- How the codebase is organized -->

## Testing Patterns
<!-- How tests work in this project -->

## Common Commands
<!-- Frequently used commands -->

## DO NOT MODIFY
<!-- Files/patterns that should never be changed -->
```

---

## 8. Create File: ralph/PROMPT.md (Enhanced for both modes)

```markdown
# RALPH LOOP ITERATION PROMPT

You are in a RALPH LOOP iteration. You have NO memory of previous iterations.

## YOUR ONLY CONTEXT SOURCES:
1. `ralph/progress.txt` - READ THIS FIRST! Contains learnings and failed approaches
2. `ralph/prd.json` - Task list with completion status  
3. `ralph/AGENTS.md` - Codebase patterns (CRITICAL for existing projects!)
4. Git history - Previous commits

## ITERATION WORKFLOW

### Step 0: Determine Project Mode
```bash
cat ralph/prd.json | jq '.mode'
```
- If "existing": Pay EXTRA attention to AGENTS.md patterns
- If "new": Standard workflow

### Step 1: Orient (DO THIS FIRST)
```bash
cat ralph/progress.txt          # Read learnings - CRITICAL!
cat ralph/prd.json | jq '.userStories[] | {id, title, passes, priority}'
cat ralph/AGENTS.md             # PATTERNS TO FOLLOW!
```

**FOR EXISTING PROJECTS:** Before making ANY changes:
- Review "EXISTING PATTERNS TO PRESERVE" in AGENTS.md
- Check "DO NOT MODIFY" section
- Understand existing architecture

### Step 2: Select Task
- Pick HIGHEST PRIORITY story where `passes: false`
- Verify all `dependsOn` tasks have `passes: true`
- If blocked, document and end iteration

### Step 3: Implement
- Work ONLY on selected task
- Follow patterns from progress.txt AND AGENTS.md
- **EXISTING PROJECTS:** Match existing code style exactly
- DO NOT repeat failed approaches (check progress.txt!)
- Keep changes focused

### Step 4: Verify
Run quality gates from prd.json:
```bash
# Execute configured typecheck, lint, test commands

# FOR EXISTING PROJECTS - ALSO RUN:
# Existing test suite to check for regressions!
```

**REGRESSION CHECK (Existing Projects):**
- Run full existing test suite
- Verify no existing functionality is broken
- If regression found: FIX IT before marking complete

### Step 5: Document

**IF TASK PASSES:**
1. Update prd.json: set `passes: true`
2. Commit: `feat(ralph): US-XXX - [title]`
3. Append to progress.txt:
```
--- Iteration: [DATE TIME] - US-XXX ---
Status: COMPLETED
Task: [title]
What was done: [description]
Patterns followed: [which existing patterns were used]
New patterns discovered: [any new patterns]
Regression check: [PASSED/any notes]
Time spent: [estimate]
Notes: [anything useful for future iterations]
```
4. Update AGENTS.md with new patterns

**IF TASK FAILS:**
1. DO NOT set passes: true
2. Append to progress.txt:
```
--- Iteration: [DATE TIME] - US-XXX ---
Status: FAILED
Task: [title]
Approaches attempted:
- [Approach 1]: [result]
- [Approach 2]: [result]
- [Approach 3]: [result]
Error logs:
[relevant errors]
Regressions caused: [if any]
Analysis: [why it failed]
Potential solutions for next iteration:
- [solution 1]
- [solution 2]
Time spent: [estimate]
```

### Step 6: Signal Status
- ALL stories pass? Output: `<promise>COMPLETE</promise>`
- More stories remain? End normally
- Blocked completely? Create `ralph/BLOCKED.md`

## RULES
1. ONE task per iteration only
2. NEVER skip quality gates
3. ALWAYS update progress.txt
4. ALWAYS read progress.txt first
5. After 3 failed approaches, document and end iteration
6. **EXISTING PROJECTS:** NEVER break existing functionality
7. **EXISTING PROJECTS:** ALWAYS match existing code patterns
```

---

## 9. Create File: ralph/GENERATE_PRD.md (Enhanced)

```markdown
# PRD GENERATION INSTRUCTIONS

## Step 0: Determine Mode

Check `ralph/IDEA.md` for the Mode field:
- **NEW**: Fresh project, no existing code
- **EXISTING**: Adding features to existing codebase

---

## FOR EXISTING PROJECTS - Do This First!

### A. Run Discovery
```bash
cat ralph/DISCOVER.md
```
Follow the discovery instructions to analyze the existing codebase.

### B. Verify AGENTS.md is Populated
Ensure AGENTS.md contains:
- [ ] Tech stack details
- [ ] File organization patterns
- [ ] Naming conventions
- [ ] Testing patterns
- [ ] "DO NOT MODIFY" list

### C. Read Feature Request
```bash
cat ralph/FEATURE_REQUEST.md
```

---

## PRD Generation Process

### 1. Analyze Input
- **NEW:** Read ralph/IDEA.md thoroughly
- **EXISTING:** Read ralph/IDEA.md + ralph/FEATURE_REQUEST.md + ralph/AGENTS.md

### 2. Ask Clarifying Questions (if needed)
Up to 5 questions about:
- Unclear requirements
- Missing details
- **EXISTING:** How new features interact with existing code

### 3. Generate ralph/PRD.md
Include:
- Project overview and goals
- **EXISTING:** Summary of existing functionality being preserved
- Detailed feature descriptions  
- Technical architecture
- **EXISTING:** Integration points with existing code
- Task breakdown

### 4. Generate/Update ralph/prd.json

**For NEW projects:** Create fresh user stories

**For EXISTING projects:**
- Set `"mode": "existing"`
- Populate `existingProject` section
- Add stories to a new feature batch
- Include "integration" tasks that connect to existing code
- Add "regression test" tasks

User stories must be:

**SMALL** - Completable in one context window
✅ "Add user avatar component using existing Button pattern"
✅ "Extend API with new /dashboard endpoint"  
✅ "Add tests for new dashboard feature"
❌ "Refactor entire frontend" (too big)
❌ "Rewrite the API" (too big, too risky)

**COMPATIBLE** - For existing projects:
✅ "Add new component following existing patterns"
❌ "Change all components to new pattern" (breaks compatibility)

**VERIFIABLE** - Clear acceptance criteria including:
- Functionality works
- **EXISTING:** No regressions in existing tests

User story format:
```json
{
  "id": "US-001",
  "title": "Brief title",
  "description": "What and why",
  "acceptanceCriteria": [
    "Verifiable criterion 1",
    "Verifiable criterion 2",
    "Tests pass",
    "No regression in existing tests"
  ],
  "priority": 1,
  "passes": false,
  "dependsOn": [],
  "integratesWith": [],
  "notes": ""
}
```

### 5. Configure Quality Gates

**For EXISTING projects, include:**
```json
"qualityGates": {
  "typecheck": "[existing typecheck command]",
  "lint": "[existing lint command]",
  "test": "[new feature tests]",
  "existingTests": "[FULL existing test suite - CRITICAL]",
  "build": "[existing build command]"
}
```

### 6. Initialize/Update Progress File
- **NEW:** Set project name and start date
- **EXISTING:** Add feature batch header, preserve existing history
```

---

## 10. Create File: ralph/start.sh (Enhanced)

```bash
#!/bin/bash
set -e
RALPH_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

echo "=========================================="
echo "  RALPH LOOP - Enhanced Status Check"
echo "=========================================="

if [ ! -f "$RALPH_DIR/prd.json" ]; then
    echo "⚠️  No prd.json found."
    echo ""
    echo "🆕 FOR NEW PROJECT:"
    echo "   1. Edit ralph/IDEA.md with Mode: NEW"
    echo "   2. New Claude session: 'Read ralph/GENERATE_PRD.md and create a PRD'"
    echo ""
    echo "📦 FOR EXISTING PROJECT:"
    echo "   1. Edit ralph/IDEA.md with Mode: EXISTING"
    echo "   2. Fill out ralph/FEATURE_REQUEST.md"
    echo "   3. New Claude session: 'Read ralph/DISCOVER.md and analyze the codebase'"
    echo "   4. Then: 'Read ralph/GENERATE_PRD.md and create user stories'"
    exit 1
fi

# Detect mode
MODE=$(cat "$RALPH_DIR/prd.json" | jq -r '.mode // "new"')

echo ""
echo "📁 Mode: $MODE"

if [ "$MODE" = "existing" ]; then
    echo "📍 Existing project - remember to check AGENTS.md patterns!"
fi

echo ""
echo "📋 Task Status:"
cat "$RALPH_DIR/prd.json" | jq -r '.userStories[] | "  [\(if .passes then "✓" else " " end)] \(.id): \(.title)"'

TOTAL=$(cat "$RALPH_DIR/prd.json" | jq '.userStories | length')
DONE=$(cat "$RALPH_DIR/prd.json" | jq '[.userStories[] | select(.passes == true)] | length')
echo ""
echo "📊 Progress: $DONE / $TOTAL tasks complete"

echo ""
echo "📝 Recent Progress:"
tail -20 "$RALPH_DIR/progress.txt" 2>/dev/null || echo "  No progress yet"

echo ""
echo "=========================================="
if [ "$MODE" = "existing" ]; then
    echo "⚠️  EXISTING PROJECT REMINDER:"
    echo "   - Read AGENTS.md for patterns to follow"
    echo "   - Run existing tests after each change"
    echo "   - Match existing code style"
    echo ""
fi
echo "Start new Claude session and say:"
echo "  'Read ralph/PROMPT.md and run one iteration'"
echo "=========================================="
```

---

## 11. Create Template Files

### ralph/templates/IDEA_TEMPLATE.md
```markdown
# PROJECT IDEA TEMPLATE

## Mode: [NEW / EXISTING]

---

## For NEW Projects

### My Idea
[What you want to build]

### Problem Being Solved
[What problem? Who is it for?]

### Key Features
- Feature 1
- Feature 2
- Feature 3

### Technical Context
- **Language/Framework:** [preference or "no preference"]
- **Target Platform:** [web, CLI, desktop, mobile, etc.]

---

## For EXISTING Projects

### Project Location
[Path to existing project]

### Current State
[What exists now]

### New Features Wanted
- Feature 1
- Feature 2

### Patterns to Preserve
[Important conventions]
```

### ralph/templates/PRD_TEMPLATE.md
```markdown
# PRD: [Project Name]

## Mode: [NEW / EXISTING]

## Overview
[What we're building / adding]

## Existing Context (if applicable)
[Summary of existing project]

## Goals
- Primary goal
- Secondary goals

## Features

### Feature 1: [Name]
**Description:** [What it does]
**Integration Points:** [For existing: what it connects to]
**Acceptance Criteria:**
- [ ] Criterion 1
- [ ] Criterion 2
- [ ] No regression in existing functionality

## Technical Architecture
[High-level approach]

## Out of Scope
[What we're NOT building]

## Risk Areas (Existing Projects)
[What could break]
```

### ralph/templates/FEATURE_TEMPLATE.md
```markdown
# FEATURE REQUEST TEMPLATE

## Feature Batch: [Name/Version]
## Date: [Date]

### Features to Add

#### Feature 1: [Name]
**User Story:** As a [user], I want [feature] so that [benefit]
**Acceptance Criteria:**
- [ ] Criterion 1
- [ ] Criterion 2

### Integration Requirements
- Must work with: [existing systems]
- Must not break: [existing functionality]

### Priority Order
1. [First]
2. [Second]
```

---

## 12. Set Permissions

```bash
chmod +x ralph/start.sh
```

---

## 13. Output Completion Message

After creating all files, display:

```
✅ RALPH LOOP ENHANCED SETUP COMPLETE!

📁 Created: ralph/ directory with all infrastructure files

🚀 QUICK START:

┌─────────────────────────────────────────────────────────────┐
│  🆕 FOR NEW PROJECTS:                                        │
├─────────────────────────────────────────────────────────────┤
│  1. Edit ralph/IDEA.md (set Mode: NEW)                      │
│  2. New session: "Read ralph/GENERATE_PRD.md and create PRD"│
│  3. New session: "Read ralph/PROMPT.md and run iteration"   │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  📦 FOR EXISTING PROJECTS:                                   │
├─────────────────────────────────────────────────────────────┤
│  1. Edit ralph/IDEA.md (set Mode: EXISTING)                 │
│  2. Fill out ralph/FEATURE_REQUEST.md                       │
│  3. New session: "Read ralph/DISCOVER.md and analyze codebase"│
│  4. Same session: "Now read ralph/GENERATE_PRD.md and create │
│     user stories for the new features"                      │
│  5. New sessions: "Read ralph/PROMPT.md and run iteration"  │
└─────────────────────────────────────────────────────────────┘

📖 Key Files:
   ralph/IDEA.md           → Project idea (NEW or EXISTING mode)
   ralph/FEATURE_REQUEST.md → New features for existing projects
   ralph/DISCOVER.md       → Codebase analysis instructions
   ralph/prd.json          → Task list with status
   ralph/progress.txt      → Learnings & history
   ralph/PROMPT.md         → Iteration instructions
   ralph/AGENTS.md         → Codebase patterns (critical for existing!)

🔄 The Loop (same for both modes):
   1. New session → Read PROMPT.md → Work on one task
   2. Task done? Mark passes:true, commit, update progress.txt
   3. Task failed? Document what was tried in progress.txt
   4. Repeat with fresh session until all tasks complete

⚠️  EXISTING PROJECT REMINDERS:
   • Always run DISCOVER.md first to populate AGENTS.md
   • Match existing code patterns exactly
   • Run full test suite after every change
   • Never break existing functionality

Ready to build something! 🎉
```

---

Now create all these files and directories.
