# The Mini Tiny Challenge — Java

![Java](https://img.shields.io/badge/Java-console_game-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Modes](https://img.shields.io/badge/Play-solo_or_two_player-5B2C83?style=flat-square)
![Focus](https://img.shields.io/badge/Focus-game_logic_%26_algorithms-0A7EA4?style=flat-square)

The original console implementation of a “game within a game”: players attempt to win an outer tic-tac-toe match, but every chosen square must first be earned by completing a randomly selected mini-game.

This Java version is the logic-first foundation for the native iOS and cross-platform Tauri renditions. It concentrates the rules, difficulty system, word datasets, CPU behavior, and replay loop in a terminal experience that makes every state transition visible.

![Image](<images/image1.png>)
## Gameplay loop

```text
choose players + difficulty
          ↓
render outer tic-tac-toe board
          ↓
active player chooses a square
          ↓
select and play a mini-game
          ↓
winner of the challenge claims the square
          ↓
check outer win/draw → switch player → repeat
```

The key design twist is that selecting a strategic square is only the first decision. The player must perform well enough in another game to keep it.

## Mini-game library

`MiniGames.java` contains challenges built around several types of reasoning:

| Category | Examples |
|---|---|
| Wordplay | unscrambling, anagrams, Wordle-style play, increasingly long spelling |
| Geography | countries and capitals, with continent-specific datasets |
| Numbers | number guessing, higher/lower ranges, sequence continuation |
| Classic games | nested tic-tac-toe and coin flip |

Difficulty influences attempt counts, word lengths, number ranges, and the simulated CPU's chance of winning. The same mini-game contract returns a boolean result, which lets the outer board treat very different challenges consistently.

## One- and two-player sessions

In a two-player game, the application alternates named players and preserves the winner of each inner challenge when assigning a square. In a solo session, the second player becomes the CPU. CPU outcomes use difficulty-weighted probability, and CPU board positions are selected from available cells.

Input loops validate player count, difficulty, row/column choices, menu selections, and replay decisions so invalid terminal input does not corrupt the game state.

## Data-driven word games

The repository includes text resources for broad English dictionaries, common-word lists, Wordle words, and geographic categories. Mini-games load these files at runtime to choose prompts and validate answers rather than hard-coding every possibility.

```text
src/Files/
├── words.txt
├── 1000topwords.txt
├── 3000topwords.txt
├── wordle.txt
└── Continents/
    ├── all countries.txt
    ├── capitals.txt
    ├── africa.txt
    ├── asia.txt
    ├── europe.txt
    ├── north america.txt
    ├── oceania.txt
    └── south america.txt
```

## Architecture

- **`Main.java`** owns setup, the outer board, player switching, win/draw detection, replay, and mini-game dispatch.
- **`MiniGames.java`** implements the challenge library, file-backed prompts, shared helpers, and difficulty-aware CPU outcomes.
- **`TicTacToeBool.java`** provides a reusable nested tic-tac-toe result.
- **`WordleBool.java` / `WordleUIBool.java`** explore Wordle logic and presentation variants.
- **`MiniGamesTester.java`** is a focused driver for exercising mini-games independently of the outer match.

Reflection-based dispatch in the main game demonstrates a flexible early approach to selecting mini-game methods at runtime. The later SwiftUI and JavaScript versions evolve this idea toward typed routing and modular files.

## Run locally

Requirements: JDK 17 or newer is recommended.

From the repository root:

```bash
javac -d out src/*.java
java -cp out Main
```

Run the focused mini-game driver with:

```bash
java -cp out MiniGamesTester
```

Open the root folder in IntelliJ IDEA if you prefer IDE build/run controls. Run from the repository root so relative paths to `src/Files` resolve correctly.

## Engineering value

The project demonstrates how to manage a large interactive state machine without a graphical framework: nested loops, reusable boolean outcomes, dynamic method selection, dataset-backed validation, error handling, CPU probability, and multiple levels of win detection.

## Skills demonstrated

Java, console UX, arrays and collections, file I/O, reflection, randomization, validation loops, modular game logic, algorithmic win detection, difficulty balancing, testing helpers, and iterative product evolution across platforms.
