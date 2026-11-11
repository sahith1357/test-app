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

## Setup

### Prerequisites

- Node.js **18.0.0 or later**, as declared in `package.json`.
- npm, which is distributed with Node.js.
- An interactive terminal for answering prompts.

There are no runtime dependencies listed in `package.json`; the implementation uses Node.js built-in modules (`node:fs/promises`, `node:path`, `node:url`, and `node:readline`). No environment variables are defined or required by the checked-in source.

### Install and run

1. Clone the repository and enter its directory:

   ```sh
   git clone <repository-url>
   cd <repository-directory>
   ```

2. Install dependencies if you add any to the project. The current manifest has none, so there is no dependency installation step required.
3. Run the configured start script:

   ```sh
   npm start
   ```

**Current checkout limitation:** As committed, `index.js` resolves its support files from `src/` and its question data from `data/`, but the repository contains those files at the root instead. Node will therefore fail to resolve the imports before starting the game. To run the application, first align the layout and references—for example, place the modules under `src/` and the JSON under `data/`, or update `index.js` to reference the root-level files. This README records the observed mismatch rather than claiming that the start command currently works.

## Usage Examples

Once the path mismatch described above is corrected, start the interactive game with:

```sh
npm start
```

The intended session flow is:

1. Choose a category from the displayed numbered list.
2. Choose all available questions, or choose 3 or 5 when that many questions exist in the category.
3. Press Enter to begin, then enter the number corresponding to each answer choice.
4. Review feedback after each answer; the game shows an explanation when the question has one.
5. See the final score and any incorrect answers, then answer `y` to play another round or `n` to exit.

The category data in `questions.json` currently contains five questions for each of the three categories. Each question provides an `options` array, a zero-based `answer` index, and an optional `explanation`. The menu displays options with one-based numbers, which the input helper converts to a zero-based index for scoring.

## Additional Details

### Project behavior and implementation

- `index.js` loads JSON data, builds category and question-count menus, and runs the replay loop.
- `input.js` wraps Node's `readline` interface in Promise-based prompt, selection, confirmation, and Enter-to-continue helpers.
- `quiz.js` shuffles each selected set of questions with Fisher–Yates, maintains progress and score, and prints results and incorrect-answer review.
- `colors.js` provides ANSI escape-code styling helpers. Color support is not configurable through package settings.
- The game selects the first `N` questions in the category data before shuffling that selection; choosing 3 or 5 therefore does not randomly sample from the entire category.

### Scripts and tests

The package defines these npm scripts:

```sh
npm start  # node index.js
npm test   # node --test
```

No test files are present in the repository tree, so the test script invokes Node's test runner without project tests currently checked in.

### Question data format

Question categories are stored under the top-level `categories` object in `questions.json`. A category has a display `name` and a `questions` list. Each question contains its prompt, choices, the index of the correct choice, and optionally an explanation. Keep the answer index aligned with the corresponding position in `options` (the first option has index `0`).

### Contribution and license

No contribution guidelines are present in the checked-in files. `package.json` declares the project license as **MIT**; consult the repository's license file if one is added, since no separate license text appears in the current file listing.
