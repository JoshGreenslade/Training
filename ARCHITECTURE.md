# Training System Architecture - Discussion + PR Based

This document explains the refactored training system architecture.

## Overview

The training system has been migrated from an issue-based approach to a discussion + PR-based approach to address several problems:

1. **Duplicate workflow runs** - Multiple workflows triggered on the same issue comment events
2. **Double-counting XP** - Same action processed multiple times
3. **Unclear completion signals** - Users unsure when to mark something complete
4. **Scattered progress** - Multiple issues per trainee made tracking difficult

## New Architecture

### 1. Training Start Flow

```
User creates issue with start-training template
    ↓
Training Orchestrator triggers (on issues.opened)
    ↓
Creates personalized discussion for user
    ↓
Closes the initial issue
    ↓
User finds their discussion: "🎓 Training Progress: @username"
```

**Key Points**:
- Issue is only used to collect initial form data
- Discussion becomes the user's training home
- All progress tracked in structured metadata block

### 2. Progress Tracking Structure

Each user gets ONE discussion with this structure:

```markdown
# 🎓 Training Progress: @username

<!-- METADATA_START
trainee: username
level: 1
xp: 150
xp_needed: 500
badges: ["AI Apprentice"]
modules_completed: ["1.1", "1.2"]
challenges_completed: []
started_date: 2026-02-15
METADATA_END -->

## 📊 Current Status
[Visual display of level, XP, progress bar]

## 🏆 Badges Earned
[List of badges]

## 📚 Completed Modules
[Checklist of completed modules]

## 🎯 Current Level: Level X
[Available modules and next challenge]

## 💬 How to Progress
[Instructions for user]
```

**Key Points**:
- Metadata block is machine-parseable (YAML-like)
- Visible sections are human-readable
- Updated in-place using `update-discussion` safe-output
- Single source of truth per trainee

### 3. Challenge Delivery - Three Types

#### Type 1: Coding Challenge (User Implements)

```
User requests Level X Challenge
    ↓
Training Orchestrator creates branch with starter code
    ↓
Opens PR with challenge description
    ↓
Labels: training-challenge, do-not-merge, coding-challenge
    ↓
User checks out branch, implements solution
    ↓
User closes PR when ready
    ↓
Challenge Review Assistant triggers (on pull_request.closed)
    ↓
Reviews code, provides feedback, awards XP
    ↓
Updates user's discussion with new progress
```

**Example Challenges**: "Create your first agent", "Build PR review workflow"

#### Type 2: Code Review Challenge (User Reviews AI Code)

```
User requests Level X Challenge
    ↓
Training Orchestrator creates branch with buggy code
    ↓
Opens PR with challenge description
    ↓
Labels: training-challenge, do-not-merge, code-review-challenge
    ↓
User reviews code, leaves comments on issues
    ↓
User closes PR when done reviewing
    ↓
Challenge Review Assistant triggers (on pull_request.closed)
    ↓
Evaluates quality of review, awards XP
    ↓
Updates user's discussion
```

**Example Challenges**: "Review this junior's code", "Find security issues"

#### Type 3: Discussion Challenge (User Submits in Discussion)

```
Training Orchestrator posts challenge in user's discussion
    ↓
User replies with their answer
    ↓
Challenge Review Assistant triggers (on discussion_comment)
    ↓
Reviews submission, provides feedback, awards XP
    ↓
Updates user's discussion
```

**Example Challenges**: "Design AI transformation strategy", "Answer theory questions"

### 4. Module Content Delivery

**Shared Module Discussions**:
- One discussion per module (e.g., "📚 Module 1.1: AI Revolution")
- Created once, referenced by all users
- Contains full learning content
- Users mark complete by replying to their training discussion

**Flow**:
```
User: "I'd like to start Module 1.1"
    ↓
Orchestrator checks if Module 1.1 discussion exists
    ↓
If not, creates it with full content
    ↓
Updates user's discussion with link to module
    ↓
User reads module
    ↓
User: "I've completed Module 1.1"
    ↓
Orchestrator awards XP, updates discussion
```

### 5. Daily Challenges

**Single Discussion Approach**:
- One discussion: "📅 Daily AI Challenges - Training Hub"
- Updated daily (not created daily)
- Current challenge at top, previous in archive

**Flow**:
```
Daily trigger fires
    ↓
Daily Challenge workflow runs
    ↓
Generates new challenge based on day of week
    ↓
Finds Daily Challenges discussion (or creates if first time)
    ↓
Updates discussion: moves current to archive, adds new
    ↓
Uses update-discussion safe-output
```

**User Participation**:
```
User replies to Daily Challenges discussion with solution
    ↓
Challenge Review Assistant sees discussion_comment
    ↓
Awards XP
    ↓
Updates user's training discussion
```

### 6. Leaderboard

**Single Leaderboard Discussion**:
- One discussion: "🏆 AI-First Training Leaderboard"
- Updated weekly
- Reads from all user training discussions

**Flow**:
```
Weekly trigger fires
    ↓
Leaderboard workflow runs
    ↓
Searches for all discussions titled "🎓 Training Progress: @*"
    ↓
Parses metadata block from each
    ↓
Calculates rankings, stats, highlights
    ↓
Generates leaderboard markdown
    ↓
Finds leaderboard discussion (or creates if first time)
    ↓
Uses update-discussion to replace content
```

## Safe-Outputs Used

| Workflow | Safe-Outputs |
|----------|-------------|
| training-orchestrator | create-discussion, update-discussion, create-pull-request, close-issue |
| challenge-review-assistant | add-comment, update-discussion |
| daily-ai-challenge | create-discussion, update-discussion |
| training-leaderboard | create-discussion, update-discussion |

**Removed**: `upload-asset` (replaced with discussion-based content)

## Workflow Triggers

| Workflow | Triggers |
|----------|----------|
| training-orchestrator | issues.opened (start-training label only), workflow_dispatch |
| challenge-review-assistant | pull_request.closed, discussion_comment |
| daily-ai-challenge | schedule.daily, workflow_dispatch |
| training-leaderboard | schedule.weekly, workflow_dispatch |

**Key Changes**:
- Removed `issue_comment` triggers (main source of duplicates)
- PR challenges use `closed` event (clear completion signal)
- Discussions use `discussion_comment` for submissions

## Benefits of New Architecture

### 1. No Duplicate Runs
- Each trigger is unique and atomic
- PR close happens once
- No race conditions from multiple comments

### 2. No Double-Counting XP
- Each event processed exactly once
- XP awarded once per challenge
- Progress updated atomically

### 3. Single Source of Truth
- One discussion per trainee
- All progress in one place
- Easy to audit and verify

### 4. Clear User Experience
- "Close PR when done" is obvious
- No ambiguous commands
- Progress always visible in discussion

### 5. Better Agent Parsing
- Structured metadata block
- Easy to read current state
- Simple to update specific fields

### 6. Less Repository Clutter
- No proliferation of issues
- Discussions are better organized
- Easier to navigate

## Migration Notes

### For Existing Trainees
If there are existing trainees when this is deployed:
1. Their old issues can remain for reference
2. Create new discussions for them via workflow_dispatch
3. Manually populate metadata from old issues
4. Link old issues in new discussion for history

### For Workflow Compilation
The `.md` files have been updated but `.lock.yml` files need regeneration:

```bash
# Install gh-aw if needed
gh extension install github/gh-aw

# Compile all workflows
gh aw compile .github/workflows/ai-first-training-orchestrator.md
gh aw compile .github/workflows/challenge-review-assistant.md
gh aw compile .github/workflows/daily-ai-challenge.md
gh aw compile .github/workflows/training-leaderboard.md

# Commit the generated .lock.yml files
git add .github/workflows/*.lock.yml
git commit -m "Compile updated workflows"
```

## Testing Checklist

Before deploying to production:

- [ ] Compile all workflows
- [ ] Test training start flow (issue → discussion creation)
- [ ] Test module request and completion
- [ ] Test coding challenge (PR creation, user implementation, review)
- [ ] Test code review challenge (PR creation, user review, evaluation)
- [ ] Test discussion challenge (posting, submission, review)
- [ ] Test daily challenge update
- [ ] Test leaderboard update
- [ ] Verify no duplicate runs
- [ ] Verify XP counting accuracy
- [ ] Test level-up flow
- [ ] Verify discussion metadata parsing

## Future Enhancements

Possible improvements:
1. GitHub App integration for better event handling
2. GraphQL API for more efficient discussion queries
3. Badge images/graphics
4. Automated module content generation
5. AI-powered personalized recommendations
6. Team-based challenges and competitions
