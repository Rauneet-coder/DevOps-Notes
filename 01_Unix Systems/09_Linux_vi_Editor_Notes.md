# Linux — The vi Editor
> Short & clear revision notes · 9 topics

---

## 01 · What is vi?

`vi` is the standard text editor built into every Unix/Linux system. An improved version called **VIM** (Vi IMproved) is also widely available.

**Why vi is the de facto standard:**
- Available on all flavours of Unix/Linux
- Consistent behaviour across systems
- Very lightweight — requires few resources
- More user-friendly than older editors like `ed` or `ex`

### Opening vi

| Command | What it does |
|---------|--------------|
| `vi filename` | Open a file (creates it if it doesn't exist) |
| `vi -R filename` | Open in **read-only** mode |
| `view filename` | Open in **read-only** mode (same as `-R`) |

> 💡 When vi opens, you see `~` on empty lines — this means those lines don't exist in the file yet. A blank-looking line without `~` contains invisible characters (space, tab, newline).

---

## 02 · The Two Modes — Most Important Concept

vi always has **exactly one active mode**. Getting this wrong is the #1 cause of confusion.

```
┌─────────────────────────────────────────────────────┐
│                   COMMAND MODE                      │
│   (default on open — navigate, delete, copy, save)  │
│                                                     │
│        press i / a / o  →  INSERT MODE              │
│        ←  press Esc  ←                              │
│                                                     │
│                   INSERT MODE                       │
│           (type text into the file)                 │
└─────────────────────────────────────────────────────┘
```

| Mode | What you can do | How to enter |
|------|----------------|--------------|
| **Command mode** | Navigate, delete, copy, save, quit | Press `Esc` (or `Esc` twice to be safe) |
| **Insert mode** | Type text into the file | Press `i`, `a`, `o`, etc. |

> ⚠️ vi is **case-sensitive**. `i` and `I` do different things. Always check your mode before typing commands.

---

## 03 · Saving & Quitting

All save/quit commands are typed in **command mode**.

| Command | What it does |
|---------|--------------|
| `:w` | Save the file (write) |
| `:q` | Quit (only works if no unsaved changes) |
| `:wq` | Save and quit |
| `ZZ` | Save and quit (fastest way) |
| `:q!` | Quit **without saving** — discard all changes |
| `:w filename2` | Save as a different filename |

> 💡 If you're unsure what mode you're in, press `Esc` twice — you'll always land in command mode.

---

## 04 · Moving Around — Navigation Commands

Must be in **command mode**. You can prefix most commands with a number — e.g. `5j` moves down 5 lines.

### Basic cursor movement

| Command | Movement |
|---------|----------|
| `h` | Left one character |
| `l` | Right one character |
| `k` | Up one line |
| `j` | Down one line |

### Word & line movement

| Command | Movement |
|---------|----------|
| `w` | Forward to next word |
| `b` | Backward to previous word |
| `0` or `\|` | Beginning of current line |
| `$` | End of current line |
| `(` | Beginning of current sentence |
| `)` | Beginning of next sentence |
| `{` | One paragraph back |
| `}` | One paragraph forward |

### File-level movement

| Command | Movement |
|---------|----------|
| `1G` | First line of file |
| `G` | Last line of file |
| `nG` | Go to line `n` |
| `:n` | Go to line `n` |
| `H` | Top of screen |
| `M` | Middle of screen |
| `L` | Bottom of screen |

### Scroll commands (Ctrl key)

| Command | Movement |
|---------|----------|
| `Ctrl+f` | Scroll forward one full screen |
| `Ctrl+b` | Scroll backward one full screen |
| `Ctrl+d` | Scroll forward half screen |
| `Ctrl+u` | Scroll backward half screen |
| `Ctrl+e` | Scroll screen up one line |
| `Ctrl+y` | Scroll screen down one line |
| `Ctrl+l` | Redraw the screen |

---

## 05 · Inserting Text — Enter Insert Mode

From command mode, use any of these to enter insert mode at different positions:

| Command | Where text is inserted |
|---------|----------------------|
| `i` | Before the cursor |
| `I` | At the beginning of the current line |
| `a` | After the cursor |
| `A` | At the end of the current line |
| `o` | New line **below** the cursor |
| `O` | New line **above** the cursor |

> 💡 Press `Esc` to return to command mode after typing.

---

## 06 · Deleting, Changing & Copying

### Deleting (command mode)

| Command | What it deletes |
|---------|----------------|
| `x` | Character under cursor |
| `X` | Character before cursor |
| `dw` | From cursor to end of word |
| `d^` | From cursor to beginning of line |
| `d$` or `D` | From cursor to end of line |
| `dd` | Entire current line |
| `2dd` | Two lines (prefix any command with a number) |

### Changing

| Command | What it does |
|---------|--------------|
| `r` | Replace one character (stays in command mode) |
| `R` | Overwrite characters until `Esc` |
| `cw` | Change word from cursor to end |
| `cc` | Clear entire line, enter insert mode |
| `s` | Replace current character, enter insert mode |
| `S` | Delete line, enter insert mode |

### Copy & Paste (Yank)

| Command | What it does |
|---------|--------------|
| `yy` | Copy (yank) current line |
| `yw` | Copy current word |
| `p` | Paste **after** cursor |
| `P` | Paste **before** cursor |

---

## 07 · Searching

### String search (whole file)

| Command | What it does |
|---------|--------------|
| `/word` | Search **forward** (downward) for `word` |
| `?word` | Search **backward** (upward) for `word` |
| `n` | Repeat search in **same** direction |
| `N` | Repeat search in **opposite** direction |

### Character search (current line only)

| Command | What it does |
|---------|--------------|
| `fc` | Move forward to character `c` on current line |
| `Fc` | Move backward to character `c` on current line |
| `tc` | Move forward to just **before** character `c` |
| `Tc` | Move backward to just **after** character `c` |

### Regex special characters in search

| Character | Meaning |
|-----------|---------|
| `^` | Start of line |
| `$` | End of line |
| `.` | Any single character |
| `*` | Zero or more of previous character |
| `[` | Start of a matching set |

### Find & Replace

```bash
:s/search/replace/g     # replace all occurrences on current line
:%s/search/replace/g    # replace all occurrences in entire file
```

---

## 08 · Advanced & Useful Commands

| Command | What it does |
|---------|--------------|
| `u` | Undo last change |
| `U` | Restore current line to original state |
| `J` | Join current line with the next |
| `~` | Toggle case of character under cursor |
| `<<` | Shift current line left |
| `>>` | Shift current line right |
| `Ctrl+G` | Show filename, current position, and total lines |
| `:f` | Show filename and current position as % |
| `:! command` | Run a shell command without leaving vi |

### Working with multiple files

| Command | What it does |
|---------|--------------|
| `:e filename` | Open another file |
| `:e #` | Toggle between two open files |
| `:n` | Go to next file (when multiple files open) |
| `:p` | Go to previous file |
| `:r file` | Read a file and insert it after current line |
| `:cd dirname` | Change working directory |

---

## 09 · `:set` — Customise vi Behaviour

Type `:set` followed by the option in command mode:

| Command | What it does |
|---------|--------------|
| `:set nu` | Show line numbers |
| `:set ic` | Ignore case when searching |
| `:set ai` | Auto-indent new lines |
| `:set noai` | Turn off auto-indent |
| `:set sw=4` | Set shift width to 4 spaces |
| `:set wm=2` | Auto word-wrap at 2 characters from margin |
| `:set ws` | Wrap search — continue from top when reaching bottom |
| `:set ro` | Set file to read-only |
| `:set term` | Print terminal type |

---

## Quick Reference Cheatsheet

```
MODES:
  Esc           → command mode (always safe to press twice)
  i             → insert before cursor
  a             → insert after cursor
  o             → new line below

SAVE & QUIT:
  :w            → save
  :q            → quit
  :wq or ZZ     → save and quit
  :q!           → quit without saving

NAVIGATE:
  h j k l       → left down up right
  w / b         → next / previous word
  0 / $         → start / end of line
  G / 1G        → last / first line
  :n            → go to line n

DELETE:
  x             → delete character
  dd            → delete line
  dw            → delete word
  D             → delete to end of line

COPY & PASTE:
  yy            → copy line
  p / P         → paste after / before

SEARCH:
  /word         → search forward
  ?word         → search backward
  n / N         → next / previous match
  :s/old/new/g  → replace on current line
  :%s/old/new/g → replace in entire file

UNDO:
  u             → undo last change
  U             → restore current line
```

---

*Linux vi Editor Notes — for revision use*
