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
