# Quiz CLI

An interactive command-line quiz game for JavaScript, Node.js, and general programming concepts. The application presents multiple-choice questions, gives immediate feedback, tracks scores, and offers a review of incorrect answers.

> **Important: the current checkout does not start as configured.** `index.js` imports modules from `./src/` and reads questions from `data/questions.json`, but all four files are currently at the repository root. As a result, `npm start` fails during module resolution until the import paths and data path or repository layout are corrected. Details are in [Setup](#setup).

## Contents

- [Project overview](#project-overview)
- [File structure](#file-structure)
- [Setup](#setup)
- [Usage](#usage)
- [Additional details](#additional-details)
- [Question data format](#question-data-format)
- [Contributing and license](#contributing-and-license)

## Project overview

Quiz CLI is a Node.js application using ES modules and built-in Node APIs. It has no third-party dependencies. The checked-in question bank contains three categories with five questions apiece.

Features implemented in the source:

- JavaScript Basics, Node.js Fundamentals, and General Programming categories.
- Numbered prompts for category, question-count, and answer selection.
- Question progress display, immediate right/wrong feedback, and explanations when supplied in the question data.
- Final score, percentage-based performance message, and review of incorrect answers.
- Replay prompt and ANSI escape-code terminal styling.

## File structure

```text
.
├── README.md       # Project documentation
├── colors.js       # ANSI styling helpers
├── index.js        # Application entry point, menu flow, and question loading
├── input.js        # Readline-based prompts and selection helpers
├── package.json    # Package metadata, Node requirement, and npm scripts
├── questions.json  # Question bank organized by category
└── quiz.js         # Quiz state, question flow, scoring, and results
```

The code imports expect a different layout (`src/` for the JavaScript modules and `data/` for the question bank). The tree above reflects the actual checked-in layout, not the paths the entry point currently expects.

## Setup

### Prerequisites

- Node.js 18 or later (the package declares `"engines": { "node": ">=18.0.0" }`).
- npm, distributed with Node.js.
- An interactive terminal.

No environment variables are required or documented. No external packages are listed in `package.json`, so dependency installation is not necessary for the current manifest.

### Get the source

```sh
git clone https://github.com/sahith1357/test-app.git
cd test-app
```

### Startup path mismatch

The `start` script runs `node index.js`, but the entry point imports `./src/input.js`, `./src/quiz.js`, and `./src/colors.js`, and tries to read `data/questions.json`. These paths are absent in the current repository: `input.js`, `quiz.js`, `colors.js`, and `questions.json` are all in the root directory. Therefore, **`npm start` currently fails before the interactive quiz can begin**.

To make it runnable, correct the paths/layout in the code (for example, align the imports and question-data path to the files' root locations) or move files to the directories the entry point expects. No such code/layout fix is included here. After fixing the mismatch, run:

```sh
npm start
```

There is no `npm install` step needed unless dependencies are added in a future change.

## Usage

After resolving the startup path mismatch, launch the app in an interactive terminal:

```sh
npm start
```

The intended flow implemented by the CLI is:

1. Enter the number for one of the three categories.
2. Select **All questions**, **3 questions**, or **5 questions** (choices are limited to counts supported by that category).
3. Press Enter to begin and enter the number of your answer choice for each question.
4. Read immediate correctness feedback and any available explanation; press Enter between questions.
5. Review your score and incorrect answers, then answer `y` to replay or `n` to exit.

> Selecting 3 or 5 takes the first 3 or 5 questions in the category's JSON order and then shuffles that selected subset. It does **not** randomly sample those counts from all questions in the category.

## Additional details

### Runtime behavior and architecture

- `index.js` loads `questions.json`, creates category and question-count menus, and manages the quiz/replay flow.
- `input.js` provides Promise-based wrappers for Node's built-in `readline` input, numbered selection, yes/no confirmation, and Enter prompts.
- `quiz.js` shuffles the provided question set using Fisher–Yates, tracks progress and score, presents feedback, and renders the results and missed-question review.
- `colors.js` applies ANSI escape sequences for terminal styling; the implementation does not rely on a third-party color package.
- Answer choices displayed to the player are numbered starting at 1; the stored `answer` field is a zero-based index.

### Scripts and tests

The package defines these scripts:

```sh
npm start  # node index.js
npm test   # node --test
```

There are no test files checked into the current repository. `npm test` invokes Node's built-in test runner, but there are currently no project tests for it to run.

## Question data format

`questions.json` stores categories under the top-level `categories` object. Each category has a display `name` and a `questions` array. Each question has a `question` string, an `options` array, a zero-based `answer` index pointing to the correct option, and optionally an `explanation` string.

Example shape:

```json
{
  "categories": {
    "sample": {
      "name": "Sample Category",
      "questions": [
        {
          "question": "Example question?",
          "options": ["First", "Second"],
          "answer": 1,
          "explanation": "The second option is correct."
        }
      ]
    }
  }
}
```

Keep each `answer` index aligned with its option's position: the first option is index `0`, the second is index `1`, and so on. Explanations are optional.

## Contributing and license

No separate contribution guide or `LICENSE` file is present in the checked-in repository. `package.json` declares the package license as MIT; the repository does not include a standalone license text in the current file listing.
