# Linux — Environment
> Short & clear revision notes · 6 topics

---

## 01 · What is the Environment?

The **environment** in Linux is defined by **environment variables** — character strings that hold values like numbers, text, filenames, or device names. Some are set by the system, some by you, and some by the shell itself.

```bash
TEST="Unix Programming"   # set a variable (no $ sign)
echo $TEST                # access it (use $ prefix)
# Output: Unix Programming
```

> 💡 Variables are set **without** `$` but accessed **with** `$`. They retain their values until you exit the shell.

---

## 02 · Shell Initialisation — Startup Files

When you log in, the shell reads two files in order to set up your environment:

```
Login
  │
  ├── 1. /etc/profile      (system-wide — applies to ALL users)
  │
  └── 2. ~/.profile        (personal — applies to YOU only)
  │
  └── Shell prompt $ appears
```

| File | Controlled by | Purpose |
|------|--------------|---------|
| `/etc/profile` | System administrator | Shell config required by all users |
| `~/.profile` | You | Your personal customisations |

> 💡 No error is shown if either file is missing — the shell simply skips it. This applies to all **Bourne-type shells**. `bash` and `ksh` also use additional files.

### Minimum things to configure in `.profile`

- The type of terminal you are using
- Directories where the shell should look for commands (`PATH`)
- Variables affecting the look and feel of your terminal

---

## 03 · Key Environment Variables

### `TERM` — Terminal Type

```bash
TERM=vt100
```

Sets what kind of terminal you're using. Usually auto-detected, but can be set manually if output looks garbled.

### `PATH` — Command Search Path

```bash
PATH=/bin:/usr/bin
```

When you type a command, the shell searches each directory in `PATH` (separated by `:`) left to right. If not found anywhere:

```bash
$ hello
hello: not found
```

### `PS1` — Primary Prompt

The characters shown as your command prompt. Fully customisable:

```bash
PS1='=>'           # changes prompt to =>
PS1="[\u@\h \w]\$"  # shows username@hostname current-dir
```

### `PS2` — Secondary Prompt

Shown when a command is **incomplete** and the shell is waiting for more input. Default is `>`.

```bash
$ PS2="secondary prompt->"
$ echo "this is a
secondary prompt-> test"
```

---

## 04 · PS1 Escape Sequences

Use these inside `PS1` to display live info in your prompt:

| Escape | What it shows |
|--------|---------------|
| `\t` | Current time — `HH:MM:SS` |
| `\d` | Current date — `Weekday Month Date` |
| `\n` | Newline |
| `\s` | Current shell environment |
| `\W` | Working directory (short name only) |
| `\w` | Full path of working directory |
| `\u` | Current user's username |
| `\h` | Hostname of the current machine |
| `\#` | Command number — increments with each new command |
| `\$` | Shows `#` if root, `$` if normal user |

> 💡 Add your `PS1` setting to `~/.profile` so it applies automatically every time you log in.

---

## 05 · Important Environment Variables

| Variable | What it holds |
|----------|---------------|
| `HOME` | Home directory of the current user — default target for `cd` |
| `PATH` | Colon-separated list of directories the shell searches for commands |
| `PWD` | Current working directory (updated by `cd`) |
| `TERM` | Display/terminal type |
| `LANG` | Default system locale (e.g. `pt_BR` = Brazilian Portuguese) |
| `TZ` | Timezone — e.g. `GMT`, `AST` |
| `UID` | Numeric user ID of the current user |
| `RANDOM` | Generates a random integer (0–32767) each time it's referenced |
| `SHLVL` | Increments each time a new bash instance starts |
| `DISPLAY` | Identifier for the display X11 programs should use |
| `IFS` | Internal Field Separator — used by the parser for word splitting |
| `LD_LIBRARY_PATH` | Directories the dynamic linker searches for shared libraries |

### Viewing variable values

```bash
echo $HOME       # /root
echo $TERM       # xterm
echo $PATH       # /usr/local/bin:/bin:/usr/bin:/home/amrood/bin
echo $DISPLAY    # (blank if no display set)
```

---

## 06 · Quick Reference

```
Setting a variable:
  NAME="value"          # no spaces around =, no $ sign

Accessing a variable:
  echo $NAME            # use $ prefix to read value

Making a variable available to child processes:
  export NAME           # export it to the environment

Common prompt customisation (add to ~/.profile):
  PS1="[\u@\h \w]\$ "  # user@host current-dir $

Check your PATH:
  echo $PATH

Add a directory to PATH:
  PATH=$PATH:/new/dir
  export PATH
```

---

*Linux Environment Notes — for revision use*
