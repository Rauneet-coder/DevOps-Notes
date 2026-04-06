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
