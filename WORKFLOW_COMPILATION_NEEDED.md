# Workflow Compilation Required

The workflow markdown files (`.md`) have been updated but the corresponding lock files (`.lock.yml`) need to be regenerated.

## To Compile Workflows

1. Install gh-aw extension if not already installed:
   ```bash
   gh extension install github/gh-aw
   ```

2. Compile the workflows:
   ```bash
   cd /home/runner/work/Training/Training
   gh aw compile .github/workflows/ai-first-training-orchestrator.md
   gh aw compile .github/workflows/challenge-review-assistant.md
   gh aw compile .github/workflows/daily-ai-challenge.md
   gh aw compile .github/workflows/training-leaderboard.md
   ```

3. Commit the generated `.lock.yml` files

## Files Modified

- `.github/workflows/ai-first-training-orchestrator.md` - Main training orchestrator
- `.github/workflows/challenge-review-assistant.md` - Challenge review and XP awarding
- `.github/workflows/daily-ai-challenge.md` - Daily challenge generator
- `.github/workflows/training-leaderboard.md` - Leaderboard tracking

## Key Changes

All workflows have been refactored to use discussions instead of issues for progress tracking:

1. **Training Orchestrator**: Now creates user discussions and closes initial issues
2. **Challenge Review**: Triggers on PR closed, updates user discussions
3. **Daily Challenges**: Updates single discussion instead of creating new issues
4. **Leaderboard**: Reads from user discussions, updates single leaderboard discussion

The `.lock.yml` files are marked with `merge=ours` in `.gitattributes` so they won't cause merge conflicts.
