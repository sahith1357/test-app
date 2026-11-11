# Quiz CLI

An interactive command-line quiz game for learning JavaScript, Node.js, and general programming concepts. It presents multiple-choice questions, gives immediate feedback and explanations, tracks a score, and reviews incorrect answers at the end of a quiz.

> **Repository layout note:** The checked-in files are at the repository root, but `index.js` imports `./src/input.js`, `./src/quiz.js`, and `./src/colors.js`, and loads `data/questions.json`. Those paths are not present in the current repository tree (`input.js`, `quiz.js`, `colors.js`, and `questions.json` are at the root). Consequently, `npm start` cannot run successfully as currently laid out. See [Setup](#setup) for details; the documentation does not assume those missing paths exist.

## Project Overview

Quiz CLI is a terminal-based learning game implemented with Node.js ES modules. The package manifest identifies it as `quiz-cli` version `1.0.0`, describes it as an interactive command-line quiz game for learning JavaScript, and declares no third-party dependencies.

Features present in the source:

- Three quiz categories: JavaScript Basics, Node.js Fundamentals, and General Programming.
- Interactive numbered menus for choosing a category and question count.
- Shuffled question order, progress display, immediate right/wrong feedback, and optional explanations.
- End-of-quiz score, performance message, and review of incorrect answers.
- ANSI terminal styling implemented in project code without an external color package.
- A replay prompt after each quiz.

## File Structure

```text
.
├── .DS_Store       # macOS Finder metadata (binary; not application source)
├── colors.js       # ANSI color helpers
├── index.js        # CLI entry point and quiz/menu orchestration
├── input.js        # Readline prompts and selection helpers
├── package.json    # Package metadata and npm scripts
├── questions.json  # Question data organized by category
└── quiz.js         # Quiz state, question flow, scoring, and results
```
