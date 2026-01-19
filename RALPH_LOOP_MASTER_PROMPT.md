You are initializing a RALPH Loop project structure. The RALPH Loop is an autonomous AI development methodology that breaks projects into small, verifiable tasks and executes them one at a time across fresh sessions, maintaining context through persistent files.

Create the following complete RALPH Loop infrastructure:

## 1. Create Directory Structure

```
./ralph/
├── IDEA.md
├── PRD.md  
├── prd.json
├── progress.txt
├── AGENTS.md
├── PROMPT.md
├── GENERATE_PRD.md
├── start.sh
├── archive/
└── templates/
    ├── IDEA_TEMPLATE.md
    └── PRD_TEMPLATE.md
```

## 2. Create File: ralph/IDEA.md

```markdown
# PROJECT IDEA

<!-- 
Write your project idea here. Be as detailed or brief as you like.
Claude will ask clarifying questions if needed before generating the PRD.
-->

## My Idea

[Write your idea here]

## Technical Context (Optional)

- **Language/Framework preferences:** 
- **Target platform(s):** 
- **Existing systems to integrate:** 
- **Constraints or requirements:** 

## Additional Notes

[Any other context]
```

## 3. Create File: ralph/prd.json

```json
{
  "project": "Project Name - Update after PRD generation",
  "branchName": "ralph/main",
  "description": "Project description - Update after PRD generation",
  "qualityGates": {
    "typecheck": "echo 'Configure typecheck command'",
    "lint": "echo 'Configure lint command'",
    "test": "echo 'Configure test command'"
  },
  "userStories": []
}
```

## 4. Create File: ralph/progress.txt

```
================================================================================
RALPH LOOP PROGRESS LOG
================================================================================
Project: [Pending PRD generation]
Started: [Pending]
================================================================================

## CODEBASE PATTERNS
<!-- 
IMPORTANT: New iterations READ THIS SECTION FIRST.
Contains patterns that work, things to avoid, and accumulated wisdom.
-->

[No patterns yet - will be populated during iterations]

================================================================================
## ITERATION LOG
================================================================================

[Awaiting first iteration]
```

## 5. Create File: ralph/AGENTS.md

```markdown
# AGENTS.md - Codebase Intelligence

This file contains patterns, conventions, and learnings discovered during RALPH Loop iterations.
**Read this file at the start of each session.**

## Project Overview
[Populated after PRD generation]

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
```

## 6. Create File: ralph/PROMPT.md (Core iteration instructions)

```markdown
# RALPH LOOP ITERATION PROMPT

You are in a RALPH LOOP iteration. You have NO memory of previous iterations.

## YOUR ONLY CONTEXT SOURCES:
1. `ralph/progress.txt` - READ THIS FIRST! Contains learnings and failed approaches
2. `ralph/prd.json` - Task list with completion status
3. `ralph/AGENTS.md` - Codebase patterns
4. Git history - Previous commits

## ITERATION WORKFLOW

### Step 1: Orient (DO THIS FIRST)
```bash
cat ralph/progress.txt          # Read learnings - CRITICAL!
cat ralph/prd.json | jq '.userStories[] | {id, title, passes, priority}'
cat ralph/AGENTS.md
```

### Step 2: Select Task
- Pick HIGHEST PRIORITY story where `passes: false`
- Verify all `dependsOn` tasks have `passes: true`
- If blocked, document and end iteration

### Step 3: Implement
- Work ONLY on selected task
- Follow patterns from progress.txt
- DO NOT repeat failed approaches (check progress.txt!)
- Keep changes focused

### Step 4: Verify
Run quality gates from prd.json:
```bash
# Execute configured typecheck, lint, test commands
```

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
Time spent: [estimate]
Patterns discovered: [any new patterns]
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
```

## 7. Create File: ralph/GENERATE_PRD.md

```markdown
# PRD GENERATION INSTRUCTIONS

Read `ralph/IDEA.md` and generate a complete Product Requirements Document.

## Process:

### 1. Analyze the Idea
- Read ralph/IDEA.md thoroughly
- Identify core problem and features
- Note technical constraints

### 2. Ask Clarifying Questions (if needed)
Up to 5 questions about unclear requirements, missing details, or ambiguous scope.

### 3. Generate ralph/PRD.md
Include:
- Project overview and goals
- Detailed feature descriptions  
- Technical architecture
- Task breakdown

### 4. Generate ralph/prd.json
Create user stories that are:

**SMALL** - Completable in one context window
✅ "Create user database schema"
✅ "Add login form component"  
✅ "Implement password validation"
❌ "Build authentication system" (too big)
❌ "Create the entire API" (too big)

**VERIFIABLE** - Clear acceptance criteria
**PRIORITIZED** - Dependencies satisfied in order

User story format:
```json
{
  "id": "US-001",
  "title": "Brief title",
  "description": "What and why",
  "acceptanceCriteria": [
    "Verifiable criterion 1",
    "Verifiable criterion 2",
    "Tests pass"
  ],
  "priority": 1,
  "passes": false,
  "dependsOn": [],
  "notes": ""
}
```

### 5. Configure Quality Gates
Set appropriate commands in prd.json qualityGates:
- JavaScript/TypeScript: npm test, npm run lint, tsc
- Python: pytest, ruff/flake8, mypy  
- Go: go test, go vet
- Other: appropriate equivalents

### 6. Initialize Progress File
Update ralph/progress.txt with project name and start date.
```

## 8. Create File: ralph/start.sh

```bash
#!/bin/bash
set -e
RALPH_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

echo "=========================================="
echo "  RALPH LOOP - Status Check"
echo "=========================================="

if [ ! -f "$RALPH_DIR/prd.json" ]; then
    echo "ERROR: No prd.json found. Generate PRD first."
    exit 1
fi

echo ""
echo "📋 Task Status:"
cat "$RALPH_DIR/prd.json" | jq -r '.userStories[] | "  [\(if .passes then "✓" else " " end)] \(.id): \(.title)"'

echo ""
echo "📝 Recent Progress:"
tail -15 "$RALPH_DIR/progress.txt" 2>/dev/null || echo "  No progress yet"

echo ""
echo "=========================================="
echo "Start new Claude session and say:"
echo "  Read ralph/PROMPT.md and run one iteration"
echo "=========================================="
```

## 9. Create Template Files

### ralph/templates/IDEA_TEMPLATE.md
```markdown
# PROJECT IDEA TEMPLATE

## My Idea
[What you want to build]

## Problem Being Solved
[What problem? Who is it for?]

## Key Features
- Feature 1
- Feature 2
- Feature 3

## Technical Context
- **Language/Framework:** [preference or "no preference"]
- **Target Platform:** [web, CLI, desktop, mobile, etc.]
- **Database:** [if needed]
- **External APIs:** [if needed]

## Success Criteria
[How do you know it's done?]
```

### ralph/templates/PRD_TEMPLATE.md
```markdown
# PRD: [Project Name]

## Overview
[What we're building]

## Goals
- Primary goal
- Secondary goals

## Features
### Feature 1: [Name]
**Description:** [What it does]
**Acceptance Criteria:**
- [ ] Criterion 1
- [ ] Criterion 2

## Technical Architecture
[High-level approach]

## Out of Scope
[What we're NOT building]
```

## 10. Set Permissions

```bash
chmod +x ralph/start.sh
```

## 11. Output Completion Message

After creating all files, display:

```
✅ RALPH LOOP SETUP COMPLETE!

📁 Created: ralph/ directory with all infrastructure files

🚀 QUICK START:

1. Write your idea:
   Edit ralph/IDEA.md with your project concept

2. Generate PRD (NEW Claude session):
   "Read ralph/GENERATE_PRD.md and create a PRD from ralph/IDEA.md"

3. Run iterations (NEW Claude session each time):
   "Read ralph/PROMPT.md and execute one RALPH iteration"

4. Check status anytime:
   ./ralph/start.sh
   
   OR
   
   cat ralph/prd.json | jq '.userStories[] | {id, title, passes}'

📖 Key Files:
   ralph/IDEA.md        → Your project idea (edit this!)
   ralph/prd.json       → Task list with status
   ralph/progress.txt   → Learnings & history
   ralph/PROMPT.md      → Iteration instructions
   ralph/AGENTS.md      → Codebase patterns

🔄 The Loop:
   1. New session → Read PROMPT.md → Work on one task
   2. Task done? Mark passes:true, commit, update progress.txt
   3. Task failed? Document what was tried in progress.txt
   4. Repeat with fresh session until all tasks complete

Ready to build something! 🎉
```

---

Now create all these files and directories.
