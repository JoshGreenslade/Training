# 👨‍🏫 Training Administrator Guide

This guide is for people who want to customize, manage, or maintain the AI-First Senior Engineer Training Program.

## 🎯 Overview

The training program consists of:
- **4 Agentic Workflows** (in `.github/workflows/`)
- **1 Issue Template** (in `.github/ISSUE_TEMPLATE/`)
- **Documentation** (in `docs/` and `README.md`)
- **GitHub Discussions** (for community interaction)

## 📁 Repository Structure

```
Training/
├── .github/
│   ├── workflows/
│   │   ├── ai-first-training-orchestrator.md
│   │   ├── ai-first-training-orchestrator.lock.yml
│   │   ├── daily-ai-challenge.md
│   │   ├── daily-ai-challenge.lock.yml
│   │   ├── training-leaderboard.md
│   │   ├── training-leaderboard.lock.yml
│   │   ├── challenge-review-assistant.md
│   │   └── challenge-review-assistant.lock.yml
│   └── ISSUE_TEMPLATE/
│       └── start-training.yml
├── docs/
│   ├── QUICK_START.md
│   ├── FAQ.md
│   ├── FUTURE_OF_ENGINEERING.md
│   ├── MODULE_1_1_AI_REVOLUTION.md
│   ├── DISCUSSION_SETUP.md
│   └── ADMIN_GUIDE.md (this file)
├── .gitattributes
└── README.md
```

## 🔧 Workflow Management

### Main Training Orchestrator

**File**: `.github/workflows/ai-first-training-orchestrator.md`

**Triggers**:
- New issues (for training form submissions)
- Issue comments (for commands like `/progress`, `/help`, etc.)
- `workflow_dispatch` (manual triggers)

**What it does**:
- Creates personalized training issues for new trainees
- Responds to commands in training issues
- Awards XP and badges
- Tracks progress
- Provides guidance and feedback

**Customization Points**:
- **XP values**: Edit the XP amounts in the markdown body
- **Level thresholds**: Change the XP required for each level
- **Modules**: Add/remove/modify learning modules
- **Challenges**: Change challenge requirements
- **Badges**: Add new achievement badges

**To modify**:
1. Edit `.github/workflows/ai-first-training-orchestrator.md`
2. Run `gh aw compile ai-first-training-orchestrator`
3. Commit both `.md` and `.lock.yml` files

### Daily Challenge Generator

**File**: `.github/workflows/daily-ai-challenge.md`

**Triggers**:
- Daily schedule (fuzzy - automatically distributed)
- `workflow_dispatch` (manual trigger)

**What it does**:
- Posts a daily challenge in Discussions
- Rotates through challenge types by day of week
- Creates engaging, quick challenges (15-30 min)

**Customization Points**:
- **Challenge types**: Modify the day-of-week themes
- **Difficulty**: Adjust complexity
- **XP rewards**: Change point values
- **Format**: Alter the challenge template

**To modify**:
1. Edit `.github/workflows/daily-ai-challenge.md`
2. Recompile: `gh aw compile daily-ai-challenge`
3. Commit changes

### Training Leaderboard

**File**: `.github/workflows/training-leaderboard.md`

**Triggers**:
- Weekly schedule (fuzzy - automatically distributed)
- `workflow_dispatch` (manual trigger)
- Issue comments (for commands like `/my-stats`)

**What it does**:
- Updates weekly leaderboard in Discussions
- Responds to stats requests
- Tracks community metrics
- Awards special badges

**Customization Points**:
- **Ranking categories**: Add/remove ranking types
- **Update frequency**: Change from weekly to daily/monthly
- **Badge criteria**: Modify achievement requirements
- **Stats displayed**: Change what metrics are shown

**To modify**:
1. Edit `.github/workflows/training-leaderboard.md`
2. Recompile: `gh aw compile training-leaderboard`
3. Commit changes

### Challenge Review Assistant

**File**: `.github/workflows/challenge-review-assistant.md`

**Triggers**:
- Pull requests (with `training-challenge` label)
- Issue comments (challenge submissions)

**What it does**:
- Reviews challenge submissions
- Provides detailed feedback
- Awards XP based on quality
- Suggests improvements

**Customization Points**:
- **Review criteria**: Change what's evaluated
- **Scoring rubric**: Adjust how XP is calculated
- **Feedback format**: Modify the response template
- **Encouragement level**: Make more/less strict

**To modify**:
1. Edit `.github/workflows/challenge-review-assistant.md`
2. Recompile: `gh aw compile challenge-review-assistant`
3. Commit changes

## 🎓 Customizing Training Content

### Adding New Modules

1. **Define the module** in the orchestrator workflow:
   ```markdown
   **Module 3.4: New Topic (150 XP)**
   Learn about:
   - Concept 1
   - Concept 2
   - Hands-on exercise
   ```

2. **Create module content** (optional):
   - Add to `docs/MODULE_3_4_NEW_TOPIC.md`
   - Link from the training issue

3. **Update XP tracking**:
   - Make sure the workflow tracks completion
   - Award appropriate XP

### Adding New Levels

To add a Level 6:

1. **Update the orchestrator**:
   ```markdown
   **Level 6: AI Innovator (8001+ XP)** 🚀
   - Badge: "AI Innovator"
   - Focus: Creating new AI paradigms
   - Challenge: Pioneer a novel AI approach
   ```

2. **Create Level 6 modules and challenge**

3. **Update leaderboard** to recognize Level 6

4. **Update README** and documentation

### Adding New Challenge Types

1. **Define in daily challenge workflow**:
   ```markdown
   ### Saturday: 🧪 Experiment Saturday
   Try experimental AI techniques
   - Post a cutting-edge scenario
   - Encourage exploration
   - Award 100 XP for completion
   ```

2. **Create challenge template**

3. **Update rotation logic** if needed

### Modifying XP Values

To rebalance XP across the program:

1. **Decide new values**:
   ```
   Modules: 50 → 75 XP
   Challenges: 300-1000 → 500-1500 XP
   Daily: 50-100 → 100-150 XP
   ```

2. **Update orchestrator workflow** with new values

3. **Update all documentation** to reflect changes

4. **Announce to active trainees**

## 🎮 Managing Active Training

### Monitoring Trainees

**View all training issues**:
```
Label: training
Sort by: Recently updated
```

**Check workflow runs**:
```
Actions tab → Filter by workflow name
```

**View discussions**:
```
Discussions → Filter by category
```

### Manual Interventions

Sometimes you need to manually help:

**Award bonus XP**:
1. Comment on training issue
2. Tag the orchestrator: "@ai-training-master, award 200 bonus XP to @username for exceptional help"
3. Manually track if needed

**Fix stuck trainee**:
1. Check their training issue
2. Look at recent comments/commands
3. Check Actions tab for errors
4. Manually comment with guidance
5. May need to trigger workflow manually

**Reset progress** (rare):
1. Close old training issue
2. Have trainee submit new training form
3. Or manually edit XP in issue comments

### Handling Errors

**Workflow failed**:
1. Check Actions tab for error details
2. Fix the workflow file
3. Recompile
4. Manually trigger for affected trainee

**Command not working**:
1. Check issue has correct labels
2. Verify workflow has proper triggers
3. Check for typos in command
4. Re-run workflow if needed

**Leaderboard outdated**:
1. Manually trigger workflow
2. Or wait for next scheduled run
3. Check for parsing errors in workflow logs

## 🔒 Security & Privacy

### Best Practices

✅ **Use safe-outputs** - Never use direct write permissions
✅ **Validate inputs** - Check trainee-provided data
✅ **Limit permissions** - Workflows have minimal required permissions
✅ **Review PRs carefully** - Training PRs are marked `do-not-merge`
✅ **Monitor workflow runs** - Check for suspicious activity

### Privacy Considerations

- **Public leaderboards**: Some trainees may not want public ranking
- **Sensitive info**: Ensure training issues don't expose private data  
- **Work examples**: Be careful with proprietary code in challenges

## 📊 Analytics & Improvement

### Metrics to Track

**Engagement**:
- How many trainees start?
- What % complete Level 1?
- What % reach Level 5?
- Daily challenge participation rate

**Quality**:
- Challenge submission quality scores
- Time to complete levels
- Help requests vs. smooth progress

**Community**:
- Discussion activity
- Peer helping frequency
- Question response time

### Improving the Program

**Quarterly reviews**:
1. Analyze completion rates
2. Identify where trainees get stuck
3. Review feedback in Discussions
4. Update struggling modules
5. Add more scaffolding where needed

**Collect feedback**:
- Create feedback discussion
- Survey trainees at completion
- Monitor help requests for patterns
- Watch for common confusion points

## 🛠️ Troubleshooting

### Common Issues

**"Workflow not triggering"**
- Check triggers in frontmatter
- Verify labels on issues/PRs
- Ensure workflows are enabled
- Check Actions tab for errors

**"Commands not working"**
- Verify exact command syntax
- Check issue has `training` label
- Ensure workflow has `issue_comment` trigger
- Wait a few minutes for processing

**"XP not updating"**
- Check if workflow ran successfully
- Look for errors in Actions tab
- Verify XP was added in comments
- May need manual correction

**"Leaderboard shows wrong data"**
- Check if all training issues are labeled
- Verify workflow parsing logic
- May need to update workflow to handle new edge cases
- Manually trigger update

## 🚀 Scaling to Multiple Teams

Want to run this across multiple teams?

1. **Fork the repository** for each team
2. **Customize** per team needs
3. **Cross-team leaderboard** (optional):
   - Aggregate stats across repos
   - Create org-wide discussion
   - Monthly inter-team competitions

4. **Shared learnings**:
   - Share successful challenges
   - Document best practices
   - Create community of practice

## 📝 Maintenance Checklist

### Weekly
- [ ] Check for stuck trainees
- [ ] Review workflow errors
- [ ] Monitor discussion questions
- [ ] Celebrate level-ups

### Monthly
- [ ] Review completion rates
- [ ] Update documentation if needed
- [ ] Check for workflow improvements
- [ ] Analyze engagement metrics

### Quarterly
- [ ] Comprehensive program review
- [ ] Update training content
- [ ] Upgrade gh-aw if needed
- [ ] Survey trainee feedback

## 🆘 Getting Help

**Workflow issues**:
- Check [gh-aw documentation](https://github.github.com/gh-aw/)
- Review examples in `.github/workflows/`
- Ask in gh-aw community

**Training content**:
- Refer to this guide
- Check other docs in `docs/`
- Adapt based on your needs

**Technical problems**:
- Check Actions tab logs
- Use `gh aw audit <run-id>` for debugging
- Review error messages carefully

## 🎉 You've Got This!

The training program is designed to be self-running, but every program needs occasional tuning. Use this guide to keep things running smoothly and continuously improve the experience.

**Questions?** Check the FAQ or create a discussion!

---

*Admin Guide v1.0 - Last updated: February 2026*
