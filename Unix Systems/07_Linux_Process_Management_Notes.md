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
