# Graphyte

Render custom patterns and text onto your GitHub contribution graph using automated commits. Each pixel becomes a real commit with a past date, translated into the graph's grid of 7 rows by 52 columns.

---

## How it works

The GitHub contribution graph is a grid: 7 rows (days of the week) by 52 columns (weeks of the year). This script reads a pattern definition file, maps each filled cell to a calendar date, and generates commits with that date. GitHub renders them as colored cells on the graph.

The result is a real commit history, not a visual hack. Each cell represents actual Git objects with valid timestamps.

---

## Requirements

- Python 3.8 or later
- Git (local installation)
- A GitHub repository to commit into
- A generated `pattern.json` file

---

## Setup

### 1. Generate a pattern

Create a `pattern.json` file that defines your design as a 7-row grid. Each string in the array represents one row. Use any character for filled cells and spaces for empty cells.

Example pattern (draws the letter "C"):

```json
[
  "  XXXXXX  ",
  " XX    XX ",
  " XX       ",
  " XX       ",
  " XX       ",
  " XX    XX ",
  "  XXXXXX  "
]
```

### 2. Create a target repository

Create a new public repository on GitHub. This is where the pattern commits will be written. An empty repository is recommended.

Clone it locally:

```bash
git clone <repository-url>
cd <repository-name>
```

### 3. Copy the script files

Copy `script.py` and `pattern.json` from this repository into your target repository. The script must be at the root of the target repository.

### 4. Push the initial setup

```bash
git add .
git commit -m "Initialize pattern commit setup"
git push origin main
```

### 5. Run the script

```bash
python script.py
```

When prompted, enter the year you want the pattern to appear on:

```
Enter year to draw pattern: 2025
```

The script will generate commits, push them to your repository, and the pattern will appear on your GitHub contribution graph.

---

## Pattern file format

The `pattern.json` file must contain an array of 7 strings, each with the same length. Each character position maps to a cell on the contribution graph:

- A space character produces an empty cell
- Any non-space character produces a filled cell

The grid alignment starts from the first Sunday of the specified year. Each string index corresponds to a day of the week (index 0 = Sunday, index 6 = Saturday). Each position within a string corresponds to a week offset from the start date.

Keep rows at 52 characters max. The contribution graph only shows 52 weeks per year, so longer rows spill into the next year.

---

## Configuration

The script supports the following constants at the top of `script.py`:

| Constant | Default | Description |
|---|---|---|
| `PATTERN_FILE` | pattern.json | Path to the pattern definition file |
| `FILE_PATH` | info.txt | File modified for each commit |
| `COMMITS_PER_PIXEL` | 5 | Number of commits per filled cell (controls shade intensity) |

Adjust `COMMITS_PER_PIXEL` to control the shade of green on the graph. Higher values produce darker cells.

---

## Disclaimer

This project generates real Git commits with custom dates. It is designed for educational and artistic purposes. Do not use it to misrepresent your activity or to gain unfair advantage. Integrity in your work matters more than the appearance of your contribution graph.

---

## License

MIT
