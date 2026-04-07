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
