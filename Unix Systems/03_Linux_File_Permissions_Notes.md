# Linux — File Permission & Access Modes
> Short & clear revision notes · 7 topics

---

## 01 · What are File Permissions?

Every file in Linux has **three levels of ownership**, and each level gets its own set of permissions:

| Level | Who it applies to |
|-------|------------------|
| **Owner** | The user who created/owns the file |
| **Group** | All users who belong to the file's group |
| **Others** | Everyone else on the system |

> 🔐 File permissions are the **first line of defence** in Linux security. They control exactly who can do what with every file.

---

## 02 · Reading Permissions — `ls -l`

Running `ls -l` shows permissions in the first column. Example:

```
-rwxr-xr-- 1 amrood users 1024 Nov 2 00:10 myfile
```

### Breaking down the permission string

```
- rwx r-x r--
│ │   │   │
│ │   │   └── Others:  read only
│ │   └─────── Group:   read + execute
│ └─────────── Owner:   read + write + execute
└───────────── File type: - = file, d = directory
```

| Position | Characters | Refers to |
|----------|-----------|-----------|
| 1 | `-` or `d` or `l` | File type |
| 2–4 | `rwx` | Owner permissions |
| 5–7 | `r-x` | Group permissions |
| 8–10 | `r--` | Others permissions |

Each position is either its letter (`r`, `w`, `x`) if allowed, or `-` if not.

---

## 03 · The Three Permissions — r, w, x

### For files

| Permission | Symbol | What it allows |
|------------|--------|----------------|
| Read | `r` | View the contents of the file |
| Write | `w` | Modify or delete the file's content |
| Execute | `x` | Run the file as a program |

### For directories

| Permission | Symbol | What it allows |
|------------|--------|----------------|
| Read | `r` | View the list of filenames inside the directory |
| Write | `w` | Add or delete files inside the directory |
| Execute | `x` | **Traverse** the directory — enter it and access its contents |

> 💡 For directories, execute is really a **traverse** permission. You need `x` on `/bin` just to run `ls` or `cd`.

---

## 04 · Changing Permissions — `chmod`

`chmod` (change mode) is used to change permissions. There are two ways to use it.

### Method 1 — Symbolic Mode

Use letters and operators to add, remove, or set permissions.

| Who | Symbol | Operators | Permission |
|-----|--------|-----------|------------|
| owner | `u` | `+` add | `r` read |
| group | `g` | `-` remove | `w` write |
| others | `o` | `=` set exactly | `x` execute |
| all three | `a` | | |

**Examples:**

| Command | What it does |
|---------|--------------|
| `chmod o+wx testfile` | Add write + execute for others |
| `chmod u-x testfile` | Remove execute from owner |
| `chmod g=rx testfile` | Set group to read + execute only |
| `chmod o+wx,u-x,g=rx testfile` | Combine multiple changes in one line |

### Method 2 — Absolute (Octal) Mode

Each permission has a numeric value. Add them up for each group (owner/group/others).

| Number | Permission | Symbol |
|--------|------------|--------|
| `0` | No permission | `---` |
| `1` | Execute | `--x` |
| `2` | Write | `-w-` |
| `3` | Write + Execute (2+1) | `-wx` |
| `4` | Read | `r--` |
| `5` | Read + Execute (4+1) | `r-x` |
| `6` | Read + Write (4+2) | `rw-` |
| `7` | All permissions (4+2+1) | `rwx` |

**Examples:**

| Command | Result | Meaning |
|---------|--------|---------|
| `chmod 755 testfile` | `-rwxr-xr-x` | Owner: all · Group: r+x · Others: r+x |
| `chmod 743 testfile` | `-rwxr---wx` | Owner: all · Group: r · Others: w+x |
| `chmod 043 testfile` | `----r---wx` | Owner: none · Group: r · Others: w+x |

> 💡 **Quick cheat:** `755` = standard for executables, `644` = standard for regular files, `777` = full access for everyone (use carefully).

---

## 05 · Changing Owner & Group

| Command | Syntax | What it does |
|---------|--------|--------------|
| `chown` | `chown user filelist` | Change the **owner** of a file |
| `chgrp` | `chgrp group filelist` | Change the **group** of a file |

**Examples:**

```bash
chown amrood testfile      # Change owner to amrood
chgrp special testfile     # Change group to 'special'
```

> ⚠️ Only the **root (superuser)** can change ownership of any file. Normal users can only change ownership of files they own.

---

## 06 · SUID & SGID Bits

Sometimes a program needs to run with **elevated permissions** temporarily — for example, `passwd` needs to write to `/etc/shadow` even though regular users can't touch that file.

This is solved with **SUID** and **SGID** bits:

| Bit | Full name | What it does |
|-----|-----------|--------------|
| **SUID** | Set User ID | Program runs with the **owner's** permissions, not the caller's |
| **SGID** | Set Group ID | Program runs with the **group owner's** permissions |

### How to spot them

The `s` letter appears in place of `x` in the execute position:

```bash
$ ls -l /usr/bin/passwd
-r-sr-xr-x 1 root bin 19031 Feb 7 13:47 /usr/bin/passwd
      ^
      s here = SUID is set (runs as root, whoever calls it)
```

> 💡 A **lowercase `s`** means the execute bit is also set. A **capital `S`** means execute is NOT set (SUID is set but the file isn't executable).

### Setting SUID/SGID

```bash
chmod ug+s dirname
# Result: drwsr-sr-x
```

---

## 07 · Sticky Bit

The **sticky bit** on a directory adds an extra layer of protection — even if others have write access to the directory, they can only delete files they own.

A file inside a sticky directory can only be removed by:

- The **owner** of the sticky directory
- The **owner** of the file being removed
- The **superuser** (root)

> 💡 The classic example is `/tmp` — everyone can write there, but no one can delete someone else's files.

---

## Quick Reference Cheatsheet

```
Permission string breakdown:
  - rwx r-x r--
  │ │   │   └── Others (8-10)
  │ │   └─────── Group  (5-7)
  │ └─────────── Owner  (2-4)
  └───────────── File type

Octal values:
  r = 4,  w = 2,  x = 1
  7 = rwx,  6 = rw-,  5 = r-x,  4 = r--

Common permission sets:
  chmod 755   →  rwxr-xr-x  (executables)
  chmod 644   →  rw-r--r--  (regular files)
  chmod 700   →  rwx------  (private to owner only)
  chmod 777   →  rwxrwxrwx  (full open — use with care)
```

---

*Linux File Permissions Notes — for revision use*
