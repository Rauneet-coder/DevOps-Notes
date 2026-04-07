# Linux & Shell Scripting for DevOps 🐧🐚

This repository serves as a centralized hub for **Linux administration notes**, **Shell (Bash) scripting concepts**, and general **DevOps practices**. It contains educational materials ranging from foundational Linux file management to automation-oriented scripts helpful for system administration, DevOps workflows, and technical interview preparation.

---

## 📌 Purpose of This Repository

- Compile and organize comprehensive **DevOps and system administration notes**
- Master **Linux command-line utilities**, file management, and permissions
- Learn and practice **Shell / Bash scripting** scenarios
- Build scripts to automate repetitive system tasks
- Create a strong foundation for **DevOps & Cloud** roles and technical interviews

---

## 🗂️ Repository Structure

```bash
.
├── Shell Scripting /  # Complete bash shell scripting notes
├── Unix Systems/      # Linux system administration and core concepts notes
└── README.md
```

---

## 📚 Documentation Index

- [01 Linux File Management Notes](#01-linux-file-management-notes)
- [02 Linux Directories Notes](#02-linux-directories-notes)
- [03 Linux File Permissions Notes](#03-linux-file-permissions-notes)
- [04 Linux Environment Notes](#04-linux-environment-notes)
- [05 Linux Printing Email Notes](#05-linux-printing-email-notes)
- [06 Linux Pipes Filters Notes](#06-linux-pipes-filters-notes)
- [07 Linux Process Management Notes](#07-linux-process-management-notes)
- [08 Linux Network Utilities Notes](#08-linux-network-utilities-notes)
- [09 Linux Vi Editor Notes](#09-linux-vi-editor-notes)

- [Linux Shell Scripting Notes Complete](#linux-shell-scripting-notes-complete)


---

## 📖 Documentation


<!-- FILE: 01_Linux_File_Management_Notes.md -->

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


---



<!-- FILE: 02_Linux_Directories_Notes.md -->

# Linux — Directories
> Short & clear revision notes · 7 topics

---

## 01 · What is a Directory?

A directory in Linux is a **special file** whose only job is to store file names and their related information. Every file — whether ordinary, special, or a directory — is contained inside a directory.

Linux organises all files and directories in a **hierarchical tree structure**. At the very top is the **root directory**, represented by a single slash `/`. Every other file and directory lives somewhere below it.

> 🏢 **Think of it like a company** — the CEO is `/` (root), departments are directories, and employees are files. Everything has a place in the hierarchy.

---

## 02 · Linux Directory Structure

These are the standard directories found directly under `/` in every Linux system:

| Directory | Purpose |
|-----------|---------|
| `/bin` | Essential binary programs (commands like `ls`, `cp`, `mv`) |
| `/boot` | Boot files, kernels, and configs needed at startup |
| `/dev` | Device files representing hardware (disks, terminals) |
| `/etc` | System configuration files and startup scripts |
| `/home` | Home directories for all regular users |
| `/lib` | Shared system libraries used by programs |
| `/media` | Auto-mounted removable media (USB, CD-ROM, cameras) |
| `/mnt` | Manually mounted filesystems and drives |
| `/opt` | Optional third-party applications |
| `/proc` | Virtual files showing running processes and system state |
| `/root` | Home directory of the root (admin) user |
| `/sbin` | System binaries — admin/maintenance commands |
| `/srv` | Files served to other systems (web, FTP data) |
| `/sys` | Virtual filesystem exposing kernel and hardware info |
| `/tmp` | Temporary files — cleared on reboot |
| `/usr` | User applications and files accessible to all users |
| `/var` | Variable data — logs, databases, mail spools |

---

## 03 · Home Directory

When you log in, Linux places you in your **home directory** — your personal space, usually at `/home/your-username`.

| Command | What it does |
|---------|--------------|
| `cd ~` | Go to your own home directory from anywhere |
| `cd ~username` | Go to another user's home directory |
| `cd -` | Go back to the previous directory you were in |

> 💡 The tilde `~` is a shortcut for your home directory path. Instead of typing `/home/rauneet`, just type `~`.

---

## 04 · Absolute vs Relative Pathnames

A **pathname** describes the location of a file or directory in the filesystem. There are two types:

- **Absolute path** — always starts from the root `/`. It gives the full address of a file, no matter where you currently are.
- **Relative path** — starts from your current location. It does not begin with `/`. Use `../` to go one level up.

| Example | Type | Meaning |
|---------|------|---------|
| `/etc/passwd` | Absolute | Starts from root, always works |
| `/users/john/notes` | Absolute | Full path from root |
| `../chem/notes` | Relative | Go up one level, then into `chem/notes` |
| `personal/res` | Relative | Go into `personal` folder from current location |

| Command | What it does |
|---------|--------------|
| `pwd` | Print current working directory — shows where you are |

> 💡 Use `pwd` (print working directory) whenever you are lost — it shows your exact location in the filesystem.

---

## 05 · Listing, Creating & Removing Directories

| Command | What it does |
|---------|--------------|
| `ls dirname` | List all files inside a specific directory |
| `mkdir dirname` | Create a new directory in the current location |
| `mkdir /tmp/test-dir` | Create a directory at an absolute path |
| `mkdir docs pub` | Create multiple directories at once |
| `mkdir -p a/b/c` | Create nested directories — makes parent dirs too |
| `rmdir dirname` | Remove an empty directory |
| `rmdir d1 d2 d3` | Remove multiple empty directories at once |

> ⚠️ `rmdir` only works on **empty directories**. If a directory has files inside, you must delete the contents first before removing it.

> 💡 `mkdir -p` is very useful — it creates the full path in one go. Without `-p`, `mkdir` will fail if the parent directory doesn't exist yet.

---

## 06 · Changing & Renaming Directories

Use `cd` to move between directories and `mv` to rename them.

| Command | What it does |
|---------|--------------|
| `cd dirname` | Move into a directory (relative or absolute path) |
| `cd /usr/local/bin` | Move into a directory using absolute path |
| `cd ../../home/user` | Go up two levels, then navigate down |
| `mv mydir yourdir` | Rename a directory from `mydir` to `yourdir` |

### Going up levels with `..`

```
cd ..        →  move one level up
cd ../..     →  move two levels up
cd ../../..  →  move three levels up  (chain as many as needed)
```

---

## 07 · The Dot (`.`) and Double Dot (`..`) Entries

Every directory in Linux contains two special hidden entries that help with navigation:

| Entry | Refers to | Common use |
|-------|-----------|------------|
| `.` | The **current directory** itself | `cp file .` — copy a file here |
| `..` | The **parent directory** — one level above | `cd ..` — go up one level |

> 💡 You can see these entries by running `ls -la` — they appear at the top of the listing with their own permissions and ownership details.

These entries exist in **every single directory** — even newly created empty ones. They are how Linux keeps track of the directory tree.

---

*Linux Directories Notes — for revision use*


---



<!-- FILE: 03_Linux_File_Permissions_Notes.md -->

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


---



<!-- FILE: 04_Linux_Environment_Notes.md -->

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


---



<!-- FILE: 05_Linux_Printing_Email_Notes.md -->

# Linux — Basic Utilities: Printing & Email
> Short & clear revision notes · 4 topics

---

## 01 · Formatting for Print — `pr`

The `pr` command does **minor formatting** of files before printing or display. It does **not** modify the original file — only changes how it looks on screen or on paper.

```bash
pr option(s) filename(s)
```

| Option | What it does |
|--------|--------------|
| `-k` | Produces `k` columns of output |
| `-d` | Double-spaces the output |
| `-h "header"` | Sets a custom report header |
| `-t` | Removes header and top/bottom margins |
| `-l PAGE_LENGTH` | Sets page length (default: 66 lines, 56 lines of text) |
| `-o MARGIN` | Offsets each line by `MARGIN` spaces |
| `-w PAGE_WIDTH` | Sets page width (default: 72 characters) |

**Example — two-column report with a header:**

```bash
$ pr -2 -h "Restaurants" food

Nov 7 9:58 1997   Restaurants   Page 1

Sweet Tooth          Isle of Java
Bangkok Wok          Big Apple Deli
Mandalay             Sushi and Sashimi
Afghani Cuisine      Tio Pepe's Peppers
```

> 💡 A common workflow: format with `pr` first, then send to the printer with `lp` or `lpr`.

---

## 02 · Printing Files — `lp` & `lpr`

These commands send a file to a **physical printer** (as opposed to the screen).

| Command | What it does |
|---------|--------------|
| `lp food` | Print file `food` on the default printer |
| `lp -n3 food` | Print 3 copies |
| `lp -dprinter food` | Print to a specific named printer |
| `lpr food` | Same as `lp` (BSD-style) |
| `lpr -3 food` | Print 3 copies (lpr style) |
| `lpr -Pprinter food` | Print to a specific printer (lpr style) |

**Example output:**

```bash
$ lp food
request id is laserp-525 (1 file)
```

> 💡 The **request ID** (e.g. `laserp-525`) is used to check status or cancel the job later.

---

## 03 · Managing the Print Queue — `lpstat`, `lpq`, `cancel`, `lprm`

### Checking the queue

| Command | What it shows |
|---------|---------------|
| `lpstat -o` | All pending print requests (ID, owner, size, time, status) |
| `lpq` | Queue with rank, owner, job number, files, and total size |

```bash
$ lpq
laserp is ready and printing
Rank    Owner   Job   Files           Total Size
active  john    573   report.ps       128865 bytes
1st     grace   574   ch03.ps         82744 bytes
2nd     john    575   standard input  23347 bytes
```

### Cancelling a job

| Command | What it does |
|---------|--------------|
| `cancel laserp-575` | Cancel a specific print job by ID |
| `cancel laserp` | Cancel whatever is currently printing on that printer |
| `lprm 575` | Cancel job number 575 (lpr style) |
| `lprm -` | Cancel **all** your jobs in the queue |

```bash
$ cancel laserp-575
request "laserp-575" cancelled
```

> ⚠️ You can only cancel your **own** jobs. Root can cancel anyone's.

---

## 04 · Sending Email — `mail`

The `mail` command sends and receives email from the terminal.

```bash
mail [-s subject] [-c cc-addr] [-b bcc-addr] to-addr
```

| Option | What it does |
|--------|--------------|
| `-s` | Specifies the subject line |
| `-c` | Sends a carbon copy (CC) — comma-separated list of addresses |
| `-b` | Sends a blind carbon copy (BCC) — comma-separated list |

### Sending a quick email

```bash
$ mail -s "Test Message" admin@yahoo.com
Hi,
This is a test
.                   ← type a lone dot to finish
Cc:                 ← add CC addresses or press Enter to skip
```

> 💡 Press **Ctrl+D** at the start of a new line, or type a single `.` on its own line to send.

### Sending a file as email body

```bash
$ mail -s "Report 05/06/07" admin@yahoo.com < demo.txt
```

Use the `<` redirect operator to send an entire file as the message body — no typing needed.

### Checking incoming mail

```bash
$ mail
no email
```

---

## Quick Reference

```
Format a file:
  pr -2 -h "Title" filename       # 2 columns with header

Print a file:
  lp filename                     # default printer
  lp -n2 filename                 # 2 copies
  lp -dprinter filename           # specific printer

Check / cancel print jobs:
  lpstat -o                       # show all jobs
  lpq                             # show queue with rank
  cancel laserp-575               # cancel by job ID
  lprm -                          # cancel all your jobs

Send email:
  mail -s "Subject" user@email.com
  mail -s "Subject" user@email.com < file.txt   # send a file
  mail                            # check inbox
```

---

*Linux Basic Utilities (Printing & Email) Notes — for revision use*


---



<!-- FILE: 06_Linux_Pipes_Filters_Notes.md -->

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


---



<!-- FILE: 07_Linux_Process_Management_Notes.md -->

# Linux — Process Management
> Short & clear revision notes · 7 topics

---

## 01 · What is a Process?

When you run any command in Linux, the system creates a **process** — an instance of a running program with its own special environment.

Every process is tracked by a unique **PID** (Process ID) — a five-digit number. No two processes share the same PID at the same time. PIDs eventually roll over and repeat once all numbers are used up.

> 💡 Every time you type a command, you start a process. Even `ls` is a process.

---

## 02 · Foreground vs Background Processes

### Foreground

- Runs **connected to your terminal** — takes input from keyboard, outputs to screen
- You **cannot run anything else** until it finishes
- Default behaviour for all commands

```bash
$ ls ch*.doc        # runs in foreground — prompt returns only after it finishes
```

### Background

- Runs **disconnected from the keyboard**
- You can keep using the terminal while it runs
- Add `&` at the end of any command to run it in the background

```bash
$ ls ch*.doc &      # runs in background
[1] 1234            # [job number] PID
```

When the background job finishes:

```bash
[1] + Done    ls ch*.doc &
$              # prompt available immediately
```

> 💡 If a background process needs keyboard input, it **pauses and waits** until you bring it to the foreground.

| | Foreground | Background |
|--|-----------|------------|
| Terminal blocked? | Yes | No |
| How to start | Just run the command | Add `&` at the end |
| Good for | Quick commands | Long-running tasks |

---

## 03 · Listing Processes — `ps`

The `ps` (process status) command shows running processes.

```bash
$ ps
  PID TTY      TIME CMD
18358 ttyp3 00:00:00 sh
18361 ttyp3 00:01:31 abiword
18789 ttyp3 00:00:00 ps
```

### `ps -f` — full listing

```bash
$ ps -f
UID     PID  PPID C STIME TTY  TIME CMD
amrood 6738  3662 0 10:23 pts/6 0:00 first_one
amrood 6739  3662 0 10:22 pts/6 0:00 second_one
amrood 3662  3657 0 08:10 pts/6 0:00 -ksh
amrood 6892  3662 4 10:51 pts/6 0:00 ps -f
```

### Columns in `ps -f`

| Column | Meaning |
|--------|---------|
| `UID` | User ID — who is running this process |
| `PID` | Process ID |
| `PPID` | Parent Process ID — who started this process |
| `C` | CPU utilisation |
| `STIME` | Process start time |
| `TTY` | Terminal associated with the process |
| `TIME` | Total CPU time used |
| `CMD` | The command that started the process |

### Other `ps` options

| Option | What it shows |
|--------|---------------|
| `-a` | All users' processes |
| `-x` | Processes with no terminal |
| `-u` | Additional info (like `-f`) |
| `-e` | Extended information |

---

## 04 · Stopping Processes — `kill`

### Foreground process

Press **Ctrl+C** to interrupt and stop it immediately.

### Background process

Find the PID with `ps`, then use `kill`:

```bash
$ kill 6738        # send termination signal to PID 6738
Terminated
```

If the process ignores a normal kill:

```bash
$ kill -9 6738     # force kill — cannot be ignored
Terminated
```

> ⚠️ `kill -9` is the nuclear option — the process gets no chance to clean up. Use it only when a normal `kill` doesn't work.

---

## 05 · Parent, Child, Zombie & Orphan Processes

### Parent & Child

Every process has a **parent**. Most commands you run have the **shell** as their parent. The `PPID` column in `ps -f` shows the parent's PID.

```
shell (PPID)
  └── command you ran (PID)  ← child process
```

### Orphan Process

When a **parent process is killed before its child**, the child becomes an orphan. Linux reassigns it to the `init` process (PID 1), which becomes its new parent.

### Zombie Process

When a child process **finishes but the parent hasn't acknowledged it yet**, the child becomes a zombie. It shows up in `ps` with state **`Z`**.

- It is dead and not using resources
- It still has an entry in the process table
- Eventually cleaned up when the parent reads the exit status

| Type | Situation | State in `ps` |
|------|-----------|----------------|
| Normal | Running as expected | `S`, `R` |
| Orphan | Parent died first | Adopted by `init` |
| Zombie | Finished but not cleaned up | `Z` |

---

## 06 · Daemon Processes

A **daemon** is a background system process that:
- Usually runs with **root permissions**
- Has **no controlling terminal** (shows `?` in the `TTY` column of `ps -ef`)
- Waits silently for something to happen (e.g. a print command, a network request)

```bash
$ ps -ef | grep "?"    # daemons show ? in TTY column
```

> 💡 Examples of daemons: print daemon (waits for print jobs), web server daemon (waits for HTTP requests), cron daemon (waits for scheduled tasks).

---

## 07 · Monitoring & Job Management

### `top` — live process monitor

```bash
$ top
```

- Updates frequently in real time
- Shows CPU usage, memory, load averages, and busy processes
- Interactive — press `q` to quit

### Job ID vs Process ID

| | Job ID | Process ID (PID) |
|--|--------|-----------------|
| What it is | Short number assigned by the shell | Unique system-wide number |
| Scope | Only within your current shell session | System-wide |
| Used for | Managing background/suspended jobs | Killing or tracking any process |
| Example | `[1]` | `6738` |

> 💡 A single **job** can contain multiple processes. Using the job ID is easier than tracking each process individually.

---

## Quick Reference

```
Run in background:
  command &

List processes:
  ps              # your processes
  ps -f           # full details
  ps -a           # all users
  top             # live monitor

Stop a process:
  Ctrl+C          # stop foreground process
  kill PID        # stop background process by PID
  kill -9 PID     # force kill (last resort)

Process states in ps:
  R  = Running
  S  = Sleeping (waiting)
  Z  = Zombie (dead but not cleaned up)
  ?  = No terminal (daemon)
```

---

*Linux Process Management Notes — for revision use*


---



<!-- FILE: 08_Linux_Network_Utilities_Notes.md -->

# Linux — Network Communication Utilities
> Short & clear revision notes · 4 topics

---

## 01 · `ping` — Check if a Host is Reachable

The `ping` command sends an **echo request** to a host on the network and waits for a reply. Used to test whether a remote machine is alive and responding.

```bash
ping hostname
ping ip-address
```

Press **Ctrl+C** to stop — it runs continuously until interrupted.

### What ping is useful for

- Checking if a remote host is up or down
- Tracking and isolating network/hardware problems
- Testing network speed and measuring latency

### Example

```bash
$ ping google.com
PING google.com (74.125.67.100) 56(84) bytes of data.
64 bytes from 74.125.67.100: icmp_seq=1 ttl=54 time=39.4 ms
64 bytes from 74.125.67.100: icmp_seq=2 ttl=54 time=39.9 ms
--- google.com ping statistics ---
22 packets transmitted, 22 received, 0% packet loss
```

### Reading the output

| Field | Meaning |
|-------|---------|
| `icmp_seq` | Packet sequence number |
| `ttl` | Time To Live — hops remaining before packet is dropped |
| `time` | Round-trip time in milliseconds (lower = faster) |
| `packet loss` | % of packets that never came back (0% = healthy) |

### If the host doesn't exist

```bash
$ ping giiiiiigle.com
ping: unknown host giiiiigle.com
```

---

## 02 · `ftp` — Transfer Files Between Machines

`ftp` (File Transfer Protocol) lets you **upload and download files** between a local and a remote machine.

```bash
ftp hostname
ftp ip-address
```

After connecting, you enter your login ID and password, then use ftp's own set of commands inside the ftp shell.

### ftp commands

| Command | What it does |
|---------|--------------|
| `put filename` | Upload one file from local → remote |
| `get filename` | Download one file from remote → local |
| `mput file list` | Upload multiple files at once |
| `mget file list` | Download multiple files at once |
| `prompt off` | Turn off per-file confirmation for mput/mget |
| `prompt on` | Turn confirmation back on |
| `dir` | List files in the current directory on the **remote** machine |
| `cd dirname` | Change directory on the **remote** machine |
| `lcd dirname` | Change directory on the **local** machine |
| `quit` | Log out and close the ftp connection |

### Example session

```bash
$ ftp amrood.com
Name: amrood
Password: ****
230 User amrood logged in.

ftp> dir               # list remote files
ftp> cd mpl            # navigate remote directory
ftp> get wave_shift    # download a file
528454 bytes received in 1.296 seconds (398.1 Kbytes/s)
ftp> quit
221 Goodbye.
```

> 💡 Files are always downloaded/uploaded to your **current local directory**. Use `lcd` to change it before transferring files.

---

## 03 · `telnet` — Log Into a Remote Machine

`telnet` lets you connect to a remote Unix machine over the network and **work on it as if you were sitting at it**.

```bash
telnet hostname
```

After connecting you get a login prompt, enter credentials, and then work in the remote shell normally. Type `logout` to disconnect.

### Example session

```bash
$ telnet amrood.com
Trying...
Connected to amrood.com.
login: amrood
Password: ****
{ do your work }
$ logout
Connection closed.
```

> ⚠️ Telnet sends data including passwords in **plain text** — it is not secure. In modern systems, **SSH** is used instead of telnet for remote login.

---

## 04 · `finger` — Look Up User Information

The `finger` command displays information about users on a local or remote host — their login name, home directory, shell, login time, and more.

```bash
finger                       # all logged-in users on local machine
finger username              # specific user on local machine
finger @hostname             # all logged-in users on remote machine
finger username@hostname     # specific user on remote machine
```

### Examples

```bash
# All users logged into local machine
$ finger
Login    Name   Tty   Idle  Login Time
amrood          pts/0       Jun 25 08:03 (62.61.164.115)

# Detailed info for a specific user
$ finger amrood
Login: amrood          Directory: /home/amrood
Shell: /bin/bash
On since Thu Jun 25 08:03 on pts/0 from 62.61.164.115
No mail.

# Users on a remote machine
$ finger @avtar.com

# Specific user on remote machine
$ finger amrood@avtar.com
```

> ⚠️ `finger` is often **disabled on remote systems** for security reasons — it can expose user information to outsiders.

---

## Quick Reference

```
ping:
  ping google.com            # check if host is reachable
  ping 192.168.1.1           # ping by IP address
  Ctrl+C                     # stop pinging

ftp:
  ftp hostname               # connect to remote machine
  get filename               # download a file
  put filename               # upload a file
  mget *.txt                 # download multiple files
  lcd /local/dir             # change local directory first
  dir                        # list remote directory
  quit                       # disconnect

telnet:
  telnet hostname            # remote login (use SSH in practice)
  logout                     # disconnect

finger:
  finger                     # local users
  finger username            # specific local user
  finger @hostname           # remote users
  finger user@hostname       # specific remote user
```

---

*Linux Network Communication Utilities Notes — for revision use*


---



<!-- FILE: 09_Linux_vi_Editor_Notes.md -->

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


---



<!-- FILE: Linux_Shell_Scripting_Notes_Complete.md -->

# Linux — Shell Scripting
> Short & clear revision notes · 18 topics

---

## 01 · What is a Shell?

A **shell** is an environment that provides an interface to the Unix system. It gathers input from you and executes programs based on that input. When a program finishes executing, it displays the output.

### Shell Types

There are two major types of shells in Unix:

| Type | Default Prompt |
|------|---------------|
| Bourne shell | `$` |
| C shell | `%` |

**Bourne shell subcategories:**

| Shell | Name |
|-------|------|
| `sh` | Bourne Shell |
| `ksh` | Korn Shell |
| `bash` | GNU Bourne-Again Shell (most common) |
| `sh` | POSIX Shell |

**C-type shells:**

| Shell | Name |
|-------|------|
| `csh` | C Shell |
| `tcsh` | TENEX/TOPS C Shell |

> 💡 The original Unix shell was written in the mid-1970s by **Stephen R. Bourne** at AT&T Bell Labs. It is usually installed as `/bin/sh` on most Unix versions.

### Shell Prompt

The `$` prompt is issued by the shell. You type a command after it and press Enter — the shell reads and executes it.

```bash
$date
Thu Jun 25 08:30:19 MST 2009
```

---

## 02 · What is a Shell Script?

A **shell script** is a plain text file containing a list of commands that are listed and executed in order. A good shell script will have comments (preceded by `#`) describing each step.

Shell scripts and functions are **interpreted** — they are not compiled.

### Creating and running a script

```bash
# 1. Create the file
vi test.sh

# 2. Make it executable
chmod +x test.sh

# 3. Run it
./test.sh
```

### The Shebang line

Every script should start with a **shebang** — it tells the system which shell to use:

```bash
#!/bin/sh       # use Bourne shell
#!/bin/bash     # use bash (most common choice)
```

> 💡 The `#` symbol is called a **hash** and `!` is called a **bang** — together they form the "shebang".

### Shell Comments

```bash
#!/bin/bash
# Author : Zara Ali
# Copyright (c) Tutorialspoint.com
# Script follows here:
pwd
ls
```

### Reading user input

```bash
#!/bin/sh
echo "What is your name?"
read PERSON
echo "Hello, $PERSON"
```

---

## 03 · Variables

### Variable Naming Rules

Variable names can only contain **letters** (a–z, A–Z), **numbers** (0–9), and **underscores** (`_`). By convention, shell variable names are in **UPPERCASE**.

**Valid names:** `_ALI`, `TOKEN_A`, `VAR_1`

**Invalid names:** `2_VAR`, `-VARIABLE`, `VAR1-VAR2`, `VAR_A!`

### Defining and Accessing Variables

```bash
NAME="Zara Ali"     # set a variable (no spaces around =)
echo $NAME          # access with $ prefix
echo "Hello, $NAME" # use inside a string
```

### Reading input from the user

```bash
echo "What is your name?"
read PERSON
echo "Hello, $PERSON"
```

### Read-only Variables

```bash
readonly NAME="Zara Ali"
NAME="Qadiri"   # Error: This variable is read only
```

### Unsetting a Variable

```bash
unset NAME      # removes the variable; cannot unset readonly variables
echo $NAME      # prints nothing
```

### Variable Types

| Type | Description |
|------|-------------|
| **Local Variables** | Present only in the current shell instance; set at command prompt |
| **Environment Variables** | Available to any child process of the shell |
| **Shell Variables** | Special variables set by the shell itself to function correctly |

---

## 04 · Special Variables

These variables are reserved for specific functions in the shell:

| Variable | Description |
|----------|-------------|
| `$0` | Filename of the current script |
| `$1`, `$2` ... | Command-line arguments (positional parameters) |
| `$#` | Total number of arguments passed |
| `$*` | All arguments as a single double-quoted string |
| `$@` | All arguments as individually double-quoted strings |
| `$?` | Exit status of the last command (`0` = success) |
| `$$` | PID of the current shell / script |
| `$!` | Process number of the last background command |

```bash
# Example: ./script.sh Zara Ali
echo $0    # ./script.sh
echo $1    # Zara
echo $2    # Ali
echo $#    # 2
```

### `$*` vs `$@`

Both refer to all command-line arguments — the difference shows when enclosed in double quotes:

- `"$*"` — treats the entire list as **one argument** with spaces between
- `"$@"` — separates the list into **individual arguments**

```bash
for TOKEN in $*
do
  echo $TOKEN
done
# Output: Zara Ali 10 Years Old (each on new line)
```

### Exit Status

```bash
$echo $?   # prints 0 if last command was successful, 1 if not
```

> 💡 Most commands return `0` on success and `1` on failure. Some return additional values for specific error types.

---

## 05 · Quotes & Escaping

### Quoting Types

| Type | Syntax | Effect |
|------|--------|--------|
| Single quotes | `'...'` | Everything inside is **literal** — no expansion at all |
| Double quotes | `"..."` | Variables and commands inside **are expanded** |
| Backslash | `\` | Escapes the **next single character** |
| Back quotes | `` `command` `` | Runs a command and uses its output |

```bash
NAME="Zara"
echo '$NAME'          # $NAME  (literal — no expansion)
echo "$NAME"          # Zara   (variable expanded)
echo "Today: `date`"  # Today: Tue Apr 07 2026
echo "I have \$1200"  # I have $1200  (escaped $ sign)
```

### Metacharacters

These characters have special meaning in the shell and must be quoted if you want them treated literally:

```
* ? [ ] ' " \ $ ; & ( ) | ^ < > newline space tab
```

### Single Quotes — quoting large groups

```bash
echo '<-$1500.**>; (update?) [y|n]'   # prints everything literally
```

### Double Quotes — what still works inside

Inside double quotes, the following still have special meaning:
- `$` — parameter substitution
- `` ` `` — command substitution
- `\$`, `\"`, `\\`, `` \` `` — escaped literal characters

### Back Quotes — command substitution

```bash
DATE=`date`
echo "Current Date: $DATE"
```

---

## 06 · Shell Substitution

### Variable Substitution with `echo -e`

The `-e` option enables interpretation of backslash escape sequences:

```bash
a=10
echo -e "Value of a is $a \n"   # prints value then newline
echo    "Value of a is $a \n"   # prints \n literally
```

### Escape Sequences (used with `echo -e`)

| Escape | Description |
|--------|-------------|
| `\\` | Backslash |
| `\a` | Alert (BEL) |
| `\b` | Backspace |
| `\c` | Suppress trailing newline |
| `\f` | Form feed |
| `\n` | New line |
| `\r` | Carriage return |
| `\t` | Horizontal tab |
| `\v` | Vertical tab |

### Command Substitution

The shell runs the command inside backticks and replaces it with the output:

```bash
DATE=`date`
echo "Date is $DATE"

USERS=`who | wc -l`
echo "Logged in users: $USERS"

UP=`date ; uptime`
echo "Uptime is $UP"
```

### Variable Substitution Forms

| Form | Description |
|------|-------------|
| `${var}` | Substitutes the value of `var` |
| `${var:-word}` | If `var` is null/unset, use `word` (var unchanged) |
| `${var:=word}` | If `var` is null/unset, set `var` to `word` |
| `${var:?message}` | If `var` is null/unset, print `message` to stderr |
| `${var:+word}` | If `var` is set, use `word` (var unchanged) |

```bash
echo ${var:-"Variable is not set"}   # uses default, var unchanged
echo ${var:="Variable is not set"}   # sets var to default
echo ${var:?"Print this message"}    # error if var is unset
```

---

## 07 · Basic Operators

### Arithmetic Operators

Use `expr` for arithmetic (spaces required around operators, `*` must be escaped):

```bash
a=10; b=20
echo `expr $a + $b`     # 30
echo `expr $a - $b`     # -10
echo `expr $a \* $b`    # 200  (escape * with \)
echo `expr $b / $a`     # 2
echo `expr $b % $a`     # 0
```

Or use double parentheses (bash):

```bash
echo $((a + b))    # 30
echo $((a * b))    # 200
```

> ⚠️ Spaces are mandatory around operators in `expr`. `2+2` is wrong — use `2 + 2`.

### Relational Operators (for numbers)

| Operator | Meaning | Example |
|----------|---------|---------|
| `-eq` | Equal to | `[ $a -eq $b ]` |
| `-ne` | Not equal to | `[ $a -ne $b ]` |
| `-gt` | Greater than | `[ $a -gt $b ]` |
| `-lt` | Less than | `[ $a -lt $b ]` |
| `-ge` | Greater than or equal | `[ $a -ge $b ]` |
| `-le` | Less than or equal | `[ $a -le $b ]` |

### Boolean Operators

| Operator | Meaning | Example |
|----------|---------|---------|
| `!` | Logical NOT | `[ ! false ]` is true |
| `-o` | Logical OR | `[ $a -lt 20 -o $b -gt 100 ]` |
| `-a` | Logical AND | `[ $a -lt 20 -a $b -gt 100 ]` |

### String Operators

| Operator | Meaning | Example |
|----------|---------|---------|
| `=` | Strings are equal | `[ $a = $b ]` |
| `!=` | Strings are not equal | `[ $a != $b ]` |
| `-z` | String is empty (zero length) | `[ -z $a ]` |
| `-n` | String is not empty | `[ -n $a ]` |
| `str` | String is not empty | `[ $a ]` |

### File Test Operators

| Operator | Meaning |
|----------|---------|
| `-f file` | File exists and is a regular file |
| `-d file` | File exists and is a directory |
| `-e file` | File exists (any type) |
| `-r file` | File is readable |
| `-w file` | File is writable |
| `-x file` | File is executable |
| `-s file` | File exists and is not empty |
| `-b file` | File is a block special file |
| `-c file` | File is a character special file |
| `-g file` | File has SGID bit set |
| `-k file` | File has sticky bit set |
| `-p file` | File is a named pipe |
| `-u file` | File has SUID bit set |

> ⚠️ Always put spaces inside `[ ]` — `[ $a -gt $b ]` not `[$a -gt $b]`.

---

## 08 · Decision Making — `if`, `elif`, `else`

Unix Shell supports these forms of `if` statement: `if...fi`, `if...else...fi`, `if...elif...else...fi`.

### Syntax

```bash
if [ condition ]; then
    # commands
elif [ other_condition ]; then
    # commands
else
    # commands
fi
```

### Examples

```bash
# Compare numbers
if [ $a -gt $b ]; then
    echo "$a is greater than $b"
fi

# Check if a file exists
if [ -f "/etc/passwd" ]; then
    echo "File exists"
else
    echo "File not found"
fi

# Check string
if [ "$NAME" = "Zara" ]; then
    echo "Welcome back!"
fi
```

### `case...esac` Statement — cleaner than multiple `elif`

Similar to `switch...case` in C/C++. Used when all branches depend on the value of a single variable.

```bash
case $DAY in
    Monday)
        echo "Start of the week" ;;
    Friday)
        echo "Almost weekend" ;;
    Saturday | Sunday)
        echo "Weekend!" ;;
    *)
        echo "Midweek" ;;
esac
```

---

## 09 · Loops

### `while` loop — runs while condition is true

```bash
COUNT=1
while [ $COUNT -le 5 ]; do
    echo "Count: $COUNT"
    COUNT=$((COUNT + 1))
done
```

### `for` loop

```bash
# Loop over a list
for VAR in value1 value2 value3; do
    echo $VAR
done

# Loop over files
for file in *.txt; do
    echo "Processing $file"
done

# Loop with a range (bash)
for i in {1..5}; do
    echo "Number $i"
done
```

### `until` loop — runs until condition becomes true (opposite of while)

```bash
COUNT=1
until [ $COUNT -gt 5 ]; do
    echo "Count: $COUNT"
    COUNT=$((COUNT + 1))
done
```

### `select` loop — for creating menus

Creates a numbered menu from a list and prompts the user to select an item.

```bash
select OPTION in "option1" "option2" "quit"; do
    echo "You selected: $OPTION"
    [ "$OPTION" = "quit" ] && break
done
```

### Nested Loops

All loops support nesting. Example — nested `while`:

```bash
#!/bin/sh
a=0
while [ "$a" -lt 10 ]   # outer loop
do
    b="$a"
    while [ "$b" -ge 0 ]  # inner loop
    do
        echo -n "$b "
        b=`expr $b - 1`
    done
    echo
    a=`expr $a + 1`
done
```

> 💡 `echo -n` suppresses the trailing newline.

---

## 10 · Loop Control

### The `break` Statement

Terminates the entire loop immediately and jumps to the code after the loop.

```bash
a=0
while [ $a -lt 10 ]; do
    echo $a
    if [ $a -eq 5 ]; then
        break        # stops at 5
    fi
    a=`expr $a + 1`
done
```

**Breaking out of nested loops:**

```bash
break n    # exits the nth enclosing loop
```

```bash
for var1 in 1 2 3; do
    for var2 in 0 5; do
        if [ $var1 -eq 2 -a $var2 -eq 0 ]; then
            break 2    # exits BOTH loops
        else
            echo "$var1 $var2"
        fi
    done
done
```

### The `continue` Statement

Skips the rest of the current iteration and moves to the next one.

```bash
NUMS="1 2 3 4 5 6 7"
for NUM in $NUMS; do
    Q=`expr $NUM % 2`
    if [ $Q -eq 0 ]; then
        echo "Even number"
        continue      # skip rest, go to next iteration
    fi
    echo "Odd number"
done
```

**Skipping in nested loops:**

```bash
continue n    # continues from the nth enclosing loop
```

### Infinite Loop

A loop runs forever if its condition is never met:

```bash
a=10
until [ $a -lt 10 ]; do   # condition never true → infinite loop
    echo $a
    a=`expr $a + 1`
done
```

### Loop Control Summary

| Command | What it does |
|---------|--------------|
| `break` | Exit the loop immediately |
| `break n` | Exit the nth enclosing loop |
| `continue` | Skip the rest of this iteration, go to next |
| `continue n` | Continue from the nth enclosing loop |

---

## 11 · Functions

Functions let you break a script into smaller, reusable logical subsections — similar to subroutines in other languages.

### Defining and Calling a Function

```bash
function_name () {
    list of commands
}

# Call it
function_name
```

```bash
Hello () {
    echo "Hello World"
}
Hello    # Hello World
```

### Passing Parameters to a Function

Parameters are accessed as `$1`, `$2`, etc. inside the function:

```bash
Hello () {
    echo "Hello World $1 $2"
}
Hello Zara Ali    # Hello World Zara Ali
```

### Returning Values from Functions

```bash
# Using return (returns a numeric exit code, captured via $?)
Hello () {
    echo "Hello $1 $2"
    return 10
}
Hello Zara Ali
echo "Return value: $?"    # Return value: 10

# Using echo to return actual data
add() {
    result=$(($1 + $2))
    echo $result
}
SUM=$(add 5 3)
echo "Sum is $SUM"         # Sum is 8
```

> 💡 `return` only returns a numeric exit code (0–255). To return actual values, use `echo` and capture with `$()`.

### Nested Functions

Functions can call each other:

```bash
number_one () {
    echo "This is the first function speaking..."
    number_two
}
number_two () {
    echo "This is now the second function speaking..."
}
number_one
```

### Removing a Function Definition

```bash
unset -f function_name
```

### Function Call from Prompt

Put function definitions in `.profile` to make them available at every login. Or source a file:

```bash
$. test.sh         # load functions from test.sh into current shell
$ number_one       # now callable directly
```

---

## 12 · Arrays

A **scalar variable** holds one value. An **array variable** can hold multiple values at the same time.

### Defining Arrays

```bash
# Method 1: assign by index
NAME[0]="Zara"
NAME[1]="Qadir"
NAME[2]="Mahnaz"
NAME[3]="Ayan"
NAME[4]="Daisy"

# Method 2: bash syntax (all at once)
NAME=(Zara Qadir Mahnaz Ayan Daisy)

# ksh syntax
set -A NAME Zara Qadir Mahnaz Ayan Daisy
```

### Accessing Array Values

```bash
echo ${NAME[0]}    # Zara     (first element)
echo ${NAME[1]}    # Qadir    (second element)

# Access ALL elements
echo ${NAME[*]}    # Zara Qadir Mahnaz Ayan Daisy
echo ${NAME[@]}    # Zara Qadir Mahnaz Ayan Daisy
```

---

## 13 · Input, Output & Redirection

Most Unix commands read from **standard input** (keyboard) and write to **standard output** (terminal) by default. Redirection lets you change these sources/destinations.

### Redirection Operators

| Operator | What it does |
|----------|--------------|
| `>` | Redirect output to a file (overwrites) |
| `>>` | Append output to a file |
| `<` | Take input from a file |
| `2>` | Redirect error output (STDERR) to a file |
| `2>&1` | Redirect STDERR to same place as STDOUT |
| `\|` | Pipe output of one command to another |
| `n > file` | Redirect stream with descriptor `n` to file |
| `n >> file` | Append stream `n` to file |
| `n >& m` | Merge output stream `n` with stream `m` |
| `n <& m` | Merge input stream `n` with stream `m` |

> 💡 File descriptor: `0` = STDIN, `1` = STDOUT, `2` = STDERR

```bash
ls -l > filelist.txt          # save output to file (overwrites)
ls -l >> filelist.txt         # append to file
sort < filelist.txt           # sort reads from file
ls /fake 2> errors.txt        # save error messages to file
command > /dev/null 2>&1      # discard ALL output (both stdout & stderr)
echo message 1>&2             # display a message on STDERR
```

### Here Document

Feeds multiple lines of input directly into a command without needing a separate file:

```bash
command << DELIMITER
line 1
line 2
line 3
DELIMITER
```

```bash
# Count lines with here document
wc -l << EOF
    This is a simple lookup program
    for good restaurants
    in Cape Town.
EOF
# Output: 3

# Print multiple lines
cat << EOF
This is a simple lookup program
for good restaurants
in Cape Town.
EOF
```

> ⚠️ The delimiter must be a **single word** with no spaces or tabs. It must appear alone on the closing line.

### Discard Output

```bash
command > /dev/null        # discard stdout only
command > /dev/null 2>&1   # discard both stdout and stderr
```

---

## 14 · Man Pages (Help System)

Unix's built-in help system. Every command has a **man page** (manual page).

### Syntax

```bash
man command
man pwd      # get help on the pwd command
man man      # get help on man itself
```

### Man Page Sections

| Section | Description |
|---------|-------------|
| NAME | Name of the command |
| SYNOPSIS | General usage and parameters |
| DESCRIPTION | What the command does |
| OPTIONS | All arguments/options |
| SEE ALSO | Related commands |
| BUGS | Known issues |
| EXAMPLES | Common usage examples |
| AUTHORS | Author of the man page |

> 💡 Man pages are the **first place to look** when you don't know how to use a command.

---

## Quick Reference

```
Create & run:
  chmod +x script.sh
  ./script.sh arg1 arg2

Variables:
  NAME="value"          # set
  echo $NAME            # use
  read VAR              # get input
  readonly VAR          # make read-only
  unset VAR             # delete

Special vars:
  $1 $2 ...   arguments
  $#          argument count
  $?          last exit status (0=ok)
  $$          script PID
  $!          last background PID

Quotes:
  'literal'             # no expansion
  "expands $VAR"        # expansion allowed
  `command`             # command substitution
  \char                 # escape one character

Substitution:
  ${var:-default}       # use default if unset
  ${var:=default}       # set to default if unset
  ${var:?message}       # error if unset

Operators:
  expr $a + $b          # arithmetic with expr
  $((a + b))            # arithmetic with (())
  [ $a -eq $b ]         # numbers equal
  [ "$s" = "hi" ]       # strings equal
  [ -f file ]           # file exists
  [ -d dir ]            # dir exists
  [ -z "$s" ]           # string is empty
  ! / -o / -a           # NOT / OR / AND

Conditions:
  if [ cond ]; then ... elif ... else ... fi
  case $VAR in val) ... ;; esac

Loops:
  for x in a b c; do ... done
  while [ cond ]; do ... done
  until [ cond ]; do ... done
  break / break n / continue / continue n

Arrays:
  ARR=(a b c)           # define
  ${ARR[0]}             # access by index
  ${ARR[*]}             # all elements

Functions:
  myfunc() { echo $1; }
  myfunc "hello"
  ret=$?                # capture return code
  unset -f myfunc       # remove function

Redirect:
  cmd > file            # save output (overwrite)
  cmd >> file           # append output
  cmd < file            # read input from file
  cmd 2> file           # save errors
  cmd > /dev/null 2>&1  # discard everything
  cmd1 | cmd2           # pipe
  cmd << EOF ... EOF    # here document

Help:
  man command           # open manual page
```

---

*Linux Shell Scripting Notes — for revision use*
