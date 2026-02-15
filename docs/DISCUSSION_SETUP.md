# 🎭 Discussion Categories Setup Guide

This document provides guidance for repository administrators on setting up GitHub Discussions to support the AI-First Training Program.

## 📋 Recommended Discussion Categories

### 1. General (Default)
- **Purpose**: General conversation, questions, community chat
- **Use**: Daily challenges, community engagement, casual discussion
- **Format**: Q&A enabled

### 2. Training Help
- **Purpose**: Get help with training, ask questions, troubleshoot
- **Use**: Technical issues, conceptual questions, guidance requests
- **Format**: Q&A enabled
- **Suggested emoji**: 🆘

### 3. Show & Tell
- **Purpose**: Share your work, solutions, innovations
- **Use**: Challenge solutions, creative workflows, success stories
- **Format**: Show & Tell format
- **Suggested emoji**: 🎨

### 4. Leaderboard
- **Purpose**: Weekly leaderboard updates and statistics
- **Use**: Automated leaderboard posts, progress tracking
- **Format**: Announcement (if available) or regular
- **Suggested emoji**: 🏆

### 5. Knowledge Base
- **Purpose**: Curated learning resources, best practices, tips
- **Use**: Collected wisdom, advanced techniques, reference materials
- **Format**: Regular
- **Suggested emoji**: 📚

### 6. Feedback & Ideas
- **Purpose**: Suggest improvements to the training program
- **Use**: Feature requests, curriculum feedback, new challenge ideas
- **Format**: Ideas format
- **Suggested emoji**: 💡

## 🎯 How Workflows Use Discussions

### Daily Challenge Workflow
- Posts to: **General**
- Format: New discussion each day
- Naming: "🎯 Daily Challenge [Day] - [Category] - [Date]"

### Leaderboard Workflow
- Posts to: **Leaderboard** (or General if not available)
- Format: Updates existing discussion weekly
- Naming: "🏆 AI-First Training Leaderboard"

### Community Engagement
- Trainees can ask questions in: **Training Help**
- Trainees can share work in: **Show & Tell**
- Trainees can suggest improvements in: **Feedback & Ideas**

## 🛠️ Setup Instructions

### For Repository Administrators

1. **Enable Discussions** (if not already enabled)
   ```
   Settings → General → Features → Check "Discussions"
   ```

2. **Create Categories**
   ```
   Discussions → Categories (gear icon) → New Category
   ```

3. **Configure Each Category**
   - Set category name
   - Add description
   - Choose format (Q&A, Show & Tell, etc.)
   - Select emoji

4. **Pin Important Discussions**
   - Pin the Leaderboard discussion once created
   - Pin "Welcome & Getting Started" discussion
   - Pin "Training Resources" discussion

5. **Set Category Descriptions**
   Use the descriptions from the "Recommended Discussion Categories" section above

## 📝 Example Welcome Discussion

Create a pinned discussion in **General** category:

```markdown
# 👋 Welcome to AI-First Senior Engineer Training!

Welcome to the community! This is where we learn, share, and grow together.

## 🎯 Getting Started

1. [Read the README](../README.md) to understand the program
2. [Check the Quick Start Guide](../docs/QUICK_START.md)
3. [Start your training](../../issues/new?template=start-training.yml)

## 💬 Using Discussions

- **General**: Daily challenges, casual chat, community updates
- **Training Help**: Stuck? Need guidance? Ask here!
- **Show & Tell**: Share your solutions, workflows, and wins
- **Leaderboard**: Weekly rankings and progress tracking

## 🎮 Daily Challenges

Look for daily challenges posted here every morning. They're optional but fun!

- 🐛 Monday: Bug Hunt
- ✍️ Tuesday: Prompt Perfection
- 🏗️ Wednesday: Workflow Design
- 🔍 Thursday: Code Review
- 🎨 Friday: Creative Challenge
- 🎓 Weekend: Knowledge Check

## 🤝 Community Guidelines

- Be supportive and encouraging
- Help others learn
- Share knowledge freely
- Ask questions (no question is dumb!)
- Celebrate each other's successes

## 🏆 Recognition

Help others and you'll earn bonus XP! The most helpful community members get recognized weekly.

---

**Ready to start?** [Click here to begin training!](../../issues/new?template=start-training.yml)
```

## 🎨 Optional Enhancements

### Discussion Templates

You can create discussion templates (like issue templates) in `.github/DISCUSSION_TEMPLATE/`:

Example: `question.md`
```yaml
---
title: "[Question] "
labels: ["question"]
---

## Your Question

[Describe your question here]

## What I've Tried

[What have you already tried?]

## Context

- Training Level: [1-5]
- Module/Challenge: [Which one?]
- Error message (if any): [Paste here]
```

### Labels for Discussions

While discussions don't use labels the same way issues do, you can organize with:
- Naming conventions: "[Level 1] Question about..."
- Category selection
- Using reactions for upvoting good content

### Moderation

Assign moderators to:
- Monitor discussions
- Move posts to appropriate categories
- Answer common questions
- Highlight great contributions

## 📊 Workflow Integration

The training workflows are designed to work with minimal setup:

✅ **Works with just "General" category**
- All workflows can post to General if specific categories don't exist
- Workflows check for category existence before posting

✅ **Enhanced with recommended categories**
- Better organization
- Easier to find content
- Clearer purpose for each space

## 🔄 Maintenance

### Weekly
- Check for unanswered questions
- Pin important discussions
- Archive outdated discussions

### Monthly
- Review category usage
- Adjust categories if needed
- Recognize top contributors

## 🎉 You're Set!

With these categories configured, your training program will have a great community space for learning and collaboration.

---

*For more information, see the [main README](../README.md) or [FAQ](FAQ.md)*
