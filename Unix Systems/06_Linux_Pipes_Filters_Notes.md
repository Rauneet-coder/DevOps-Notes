# Linux — Pipes and Filters
> Short & clear revision notes · 4 topics

---

## 01 · What are Pipes and Filters?

A **pipe** connects two or more commands so that the output of one becomes the input of the next. Use the vertical bar `|` to create a pipe.

```bash
command1 | command2 | command3
```

A **filter** is any program that:
- Takes input from another program (stdin)
- Performs some operation on it
- Writes the result to stdout

> 💡 Think of a pipe like a physical pipe — data flows through it from left to right, each command processing it along the way.

---

## 02 · `grep` — Search for Patterns

`grep` searches files (or piped input) for lines matching a pattern and prints them.

```bash
grep pattern file(s)
```

> 💡 The name comes from the old Unix editor command `g/re/p` — **g**lobally search for a **r**egular **e**xpression and **p**rint matching lines.

### grep options

| Option | What it does |
|--------|--------------|
| `-v` | Print all lines that do **not** match the pattern |
| `-n` | Print matching lines with their **line numbers** |
| `-l` | Print only the **filenames** that contain matches |
| `-c` | Print only the **count** of matching lines |
| `-i` | **Case-insensitive** match (upper or lowercase) |

### Examples

```bash
# Find all files modified in August
$ ls -l | grep "Aug"
-rw-rw-rw- 1 john doc 11008 Aug  6 14:10 ch02
-rw-rw-rw- 1 john doc  8515 Aug  6 15:30 ch07
-rw-rw-r-- 1 john doc  2488 Aug 15 10:51 intro
-rw-rw-r-- 1 carol doc 1605 Aug 23 07:35 macros

# Case-insensitive search using a regular expression
# Find lines with "carol" followed by anything, then "aug"
$ ls -l | grep -i "carol.*aug"
-rw-rw-r-- 1 carol doc 1605 Aug 23 07:35 macros
```

> 💡 `.*` in a regular expression means "any character, zero or more times" — very useful for flexible pattern matching.

---

## 03 · `sort` — Sort Lines of Text

The `sort` command arranges lines of text alphabetically by default.

```bash
sort filename
```

### sort options

| Option | What it does |
|--------|--------------|
| `-n` | Sort **numerically** (so 10 comes after 9, not after 1) |
| `-r` | **Reverse** the sort order |
| `-f` | Sort upper and lowercase **together** (case-insensitive) |
| `+x` | Ignore the first `x` fields when sorting |

### Example

```bash
$ sort food
Afghani Cuisine
Bangkok Wok
Big Apple Deli
Isle of Java
Mandalay
Sushi and Sashimi
Sweet Tooth
Tio Pepe's Peppers
```

---

## 04 · Chaining Pipes — Putting it all Together

You can chain **as many commands as you need** in a single pipe. Each command processes the output of the one before it.

### Example — filter, sort, and paginate in one line

```bash
$ ls -l | grep "Aug" | sort +4n | more
```

Breaking it down step by step:

```
ls -l          →  list all files with details
  │
  ▼
grep "Aug"     →  keep only files modified in August
  │
  ▼
sort +4n       →  sort by the 5th field (file size) numerically
  │
  ▼
more           →  display one screenful at a time
```

**Output:**
```
-rw-rw-r-- 1 carol doc  1605 Aug 23 07:35 macros
-rw-rw-r-- 1 john  doc  2488 Aug 15 10:51 intro
-rw-rw-rw- 1 john  doc  8515 Aug  6 15:30 ch07
-rw-rw-rw- 1 john  doc 11008 Aug  6 14:10 ch02
--More--(74%)
```

> 💡 `sort +4n` skips the first 4 fields (separated by spaces) and sorts the rest numerically — so it sorts by file size, which is field 5 in `ls -l` output.

### `more` and `pg` — Paginate long output

| Command | What it does |
|---------|--------------|
| `more` | Pauses output when the screen is full — press Space to continue |
| `pg` | Similar to `more` — pauses and waits for a command |

---

## Quick Reference

```
Basic pipe:
  command1 | command2

grep:
  grep "word" file         # find lines with "word"
  grep -i "word" file      # case-insensitive
  grep -v "word" file      # lines NOT matching
  grep -n "word" file      # show line numbers
  grep -c "word" file      # count matches only
  ls -l | grep "Aug"       # pipe into grep

sort:
  sort file                # alphabetical
  sort -n file             # numerical
  sort -r file             # reverse
  sort +4n file            # sort by field 5, numerically

Chained pipe example:
  ls -l | grep "Aug" | sort +4n | more
```

---

*Linux Pipes and Filters Notes — for revision use*
