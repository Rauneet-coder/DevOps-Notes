# Linux — File Management
> Short & clear revision notes · 8 topics

---

## 01 · What is File Management?

In Linux, **everything is a file** — documents, programs, and even hardware devices. File management means creating, viewing, editing, copying, moving, and deleting these files.

All files live inside **directories** arranged in a tree structure starting from `/` (the root). This entire structure is called the **filesystem**.

> 📚 **Think of it like a library** — the building is the filesystem, shelves are directories, and books are files. Every file has a fixed location you can navigate to.

---

## 02 · Types of Files

Linux has three basic types: **ordinary files** (text, images, scripts), **directories** (folders), and **special files** (hardware/communication). When you run `ls -l`, the first character shows the file type:

| Symbol | File Type | Example |
|--------|-----------|---------|
| `-` | Regular file | text, scripts, images, binaries |
| `d` | Directory | a folder containing other files |
| `l` | Symbolic link | a shortcut pointing to another file |
| `b` | Block device | hard drive, USB drive |
| `c` | Character device | keyboard, modem |
| `p` | Named pipe | lets two programs share data |
| `s` | Socket | used for network/app communication |

---

## 03 · Listing Files — `ls`

| Command | What it does |
|---------|--------------|
| `ls` | List all files in the current directory |
| `ls -l` | Detailed view: type, permissions, owner, size, date |
| `ls -a` | Show hidden files too (files starting with a dot) |
| `ls *.doc` | Only `.doc` files — `*` matches any characters |
| `ls ch?.doc` | Match `ch1.doc`, `chA.doc` — `?` matches exactly one character |

### The 7 columns in `ls -l`

```
1            2       3      4      5     6              7
type+perms   links   owner  group  size  last-modified  filename
-rw-r--r--   1       rauneet users  116   Apr 06 10:00   notes.txt
```

| # | Column | Meaning |
|---|--------|---------|
| 1 | type + permissions | file type + who can read/write/execute |
| 2 | link count | number of hard links to this file |
| 3 | owner | user who owns the file |
| 4 | group | group the file belongs to |
| 5 | size | file size in bytes |
| 6 | last modified | date and time of last change |
| 7 | file name | name of the file |

---

## 04 · Hidden Files

Any file whose name starts with a `.` (dot) is **hidden** — it won't appear in a normal `ls`. These are usually system configuration files. Use `ls -a` to reveal them.

| File | Purpose |
|------|---------|
| `.profile` | Runs on login — Bourne shell startup config |
| `.kshrc` | Korn shell startup config |
| `.cshrc` | C shell startup config |
| `.rhosts` | Remote shell access config |

> 💡 In every directory: `.` means the **current directory** and `..` means the **parent directory** (one level up).

---

## 05 · Creating & Editing Files — `vi`

`vi` (or `vim`) is a text editor built into every Linux system. It has **two modes** — you must switch between them to type or navigate. It **always opens in normal mode**, so press `i` before you start typing.

| Key / Command | What happens |
|---------------|--------------|
| `vi filename` | Open file (creates it if it doesn't exist) |
| `i` | Switch to **insert mode** — now you can type |
| `Esc` | Go back to **normal mode** (for navigation) |
| `Shift + ZZ` | Save the file and exit vi |
| `h` / `l` | Move cursor left / right |
| `k` / `j` | Move cursor up / down |

### Quick workflow

```
vi notes.txt  →  press i  →  type content  →  press Esc  →  press Shift+ZZ
  (open)         (insert)     (write)          (normal)       (save & quit)
```

---

## 06 · Viewing File Content — `cat` & `wc`

Use `cat` to read a file and `wc` to count its lines, words, and bytes.

| Command | What it does |
|---------|--------------|
| `cat filename` | Print the entire file contents on screen |
| `cat -b filename` | Same, but with line numbers shown |
| `wc filename` | Shows: lines · words · bytes · filename |
| `wc f1 f2 f3` | Count stats for multiple files at once |

> **Example output:** `2 22 116 notes.txt` → 2 lines, 22 words, 116 bytes

---

## 07 · Copy, Rename & Delete Files

| Command | What it does |
|---------|--------------|
| `cp src dest` | Copy a file — the original stays untouched |
| `mv old new` | Rename a file — old name no longer exists |
| `mv file /path/` | Move a file to a different directory |
| `rm filename` | Permanently delete a file |
| `rm f1 f2 f3` | Delete multiple files at once |
| `rm -i filename` | Ask for confirmation before deleting (safer) |

> ⚠️ **There is NO Recycle Bin in Linux.** Once deleted with `rm`, a file is **gone forever**. Always prefer `rm -i` until you are confident.

---

## 08 · Links — Hard Link vs Symbolic Link

A **link** is an alternate name or path to access the same file. Linux supports two types:

| | Hard Link | Symbolic Link (Soft Link) |
|--|-----------|--------------------------|
| **What it is** | Another name for the exact same file data | A shortcut pointing to the original by path |
| **If original deleted** | File still exists (data stays) | Symlink **breaks** (points to nothing) |
| **Analogy** | Two equal front doors to the same room | A signpost pointing to the room |
| **Command** | `ln file hardlink` | `ln -s file symlink` |

> 💡 Deleting a link (either type) **never deletes the original file**. You can edit a file through its link just like through its real name.

---

*Linux File Management Notes — for revision use*
