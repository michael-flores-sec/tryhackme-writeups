# TryHackMe: The Game

| | |
|---|---|
| **Platform** | TryHackMe |
| **Room** | [The Game](https://tryhackme.com/room/hfb1thegame) |
| **Difficulty** | Easy |
| **Category** | Game Hacking |
| **Origin** | Hackfinity Battle 2025 CTF |

## The Challenge

> Cipher has gone dark, but intel reveals he's hiding critical secrets inside Tetris. Hack it and uncover the encrypted data buried in its code.

The goal: find the flag hidden inside a Tetris game.

## First Impressions

At first this room felt like it wasn't at my level. Looking back, the problem wasn't that it was too hard. I just didn't know where to start, and that threw me off.

## My Approach

### 1. Try the obvious thing first

Since the challenge said "hack the game," I started by playing it. I wanted to see if I could exploit it through gameplay, so I tried things like losing on purpose to see what would happen. That didn't lead anywhere, so I switched tactics and went to the terminal.

### 2. Find the file

I downloaded the task files inside the AttackBox. I struggled a bit to find the file in the terminal. At one point I moved out of the folder with `cd ..` and then couldn't figure out why my commands couldn't see it. Going back into the right folder fixed it:

```bash
cd /root/Downloads
ls
```

I also tried `apt install` on the zip, which doesn't work. `apt` installs software from the internet; it doesn't open files you already have.

### 3. Unzip it

The download was a `.zip`, so I extracted it:

```bash
unzip Tetrix.exe-1741979048280.zip
```

This gave me `Tetrix.exe` (plus a `__MACOSX` folder, which is just junk a Mac adds to zips).

### 4. Identify the file

```bash
file Tetrix.exe
```

```
Tetrix.exe: PE32+ executable (GUI) x86-64 (stripped to external PDB), for MS Windows, 13 sections
```

So it's a 64-bit Windows program. It won't run on the Linux AttackBox, but I didn't need to run it, just look inside it.

### 5. Pull out the readable text

I had to look up this step. Compiled programs still contain readable text, and `strings` extracts it:

```bash
strings Tetrix.exe
```

This printed a huge amount of output, way too much to read by hand.

### 6. Search for the flag

TryHackMe flags start with `THM{`, so I piped the output into `grep` to filter it:

```bash
strings Tetrix.exe | grep THM
```

That returned the flag:

```
THM{REDACTED}
```

## What I Learned

**Commands**
- `unzip`: extract a zipped file
- `file`: identify what a file actually is
- `strings`: pull readable text out of any file, even a compiled program
- `grep`: search text for a pattern
- `|` (pipe): send one command's output into another

**Habit:** Keep it simple from the beginning, then move to more complex techniques. The flag here didn't need any real game hacking, just checking what was already inside the file.

**Mindset:** Have fun with it and enjoy the challenge. Think outside the box to get around the barriers between you and the flag. "I don't know where to start" isn't the same as "I can't do this."
