# Spanish Study Materials Site Design

## Overview

A Quarto-based static site section on grmooney.com for organizing Spanish tutoring materials and personal study notes.

## Goals

- Gather Google Doc links and external resources from tutoring sessions
- Add personal notes and learnings for each lesson
- Browse materials by date, category (grammar, vocabulary, etc.), or topic (travel, food, etc.)

## Content Structure

Each lesson is a markdown file: `lessons/YYYY-MM-DD-topic.qmd`

```markdown
---
title: "Lesson Title"
date: YYYY-MM-DD
categories: [grammar, vocabulary, listening, conversation]
topics: [travel, food, daily-routines, work]
---

## Resources
- [Class notes](https://docs.google.com/...)
- [Homework](https://docs.google.com/...)
- [External resource](https://...)

## Theory
Key grammar concepts covered in class.

## Homework Learnings
What clicked (or didn't) while doing the exercises.

## Class Discussion
What we talked about, examples, corrections received.

## New Phrases
- "¿Qué tal?" - How's it going? (informal)

## Vocabulary
| Spanish | English | Notes |
|---------|---------|-------|
| el vuelo | flight | |
```

## Site Navigation

- **Home** - Intro + recent lessons
- **All Lessons** - Chronological list
- **By Category** - Grammar, Vocabulary, Listening, Conversation
- **By Topic** - Travel, Food, Daily Routines, Work, etc.

Quarto listing pages filter content automatically via frontmatter tags.

## Workflow

**Adding a new lesson:**
1. Create `lessons/YYYY-MM-DD-topic.qmd`
2. Add Google Doc links to Resources section
3. Write notes in each section
4. Commit and push (auto-deploys via GitHub Actions)

**Initial population:**
- Manual one-time effort: review Gmail for links, create lesson files
- Batch by approximate date if exact dates unknown

## Deployment

- GitHub Pages via Quarto's built-in `quarto publish gh-pages` or GitHub Actions
- Site lives at grmooney.com/spanish (or grmooney.github.io/spanish)

## Out of Scope

- Flashcard functionality
- Search
- Other sections of the personal site

## Design Principles

Following Sean Goedecke's system design philosophy:
- Keep it simple: static site with markdown files
- Use boring, well-tested components: Quarto, GitHub Pages
- Content stays in Google Docs; site aggregates links + notes
- Complexity can evolve later if needed
