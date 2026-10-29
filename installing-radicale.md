# Installing Radicale

By the end of this chapter Radicale
3.9.0 is installed in its own Python
virtual environment, a configuration
file exists that you can explain line
by line, and the server answers its
first requests — including refusing a
wrong password. The lab ran on Ubuntu
24.04 with Python 3.12; the steps are
the project's own recommended shape
(a virtual environment, then `pip`).

## Step 1: the virtual environment

A *virtual environment* (venv) is a
private folder of Python packages, so
Radicale's dependencies never tangle
with the rest of the system:

```bash
mkdir radicale-lab-work && cd radicale-lab-work
python3 -m venv venv
./venv/bin/pip install "radicale[bcrypt]"
```

The `[bcrypt]` extra matters: it pulls
in the password-hashing library the
next chapter uses to store passwords
properly. The install brought Radicale
itself, its date and calendar parsers,
and the hashing library (transcript
t00). Check what you got:

```bash
./venv/bin/radicale --version
# 3.9.0
```

## Step 2: the configuration file

Radicale's defaults are sensible but
silent; a real server deserves an
explicit config. The lab's is fifteen
lines (`radicale-lab/lab-config.ini`):

```ini
[server]
hosts = 127.0.0.1:5232

[auth]
type = htpasswd
htpasswd_filename = /home/hatch/workspace/radicale-lab-work/htpasswd
htpasswd_encryption = bcrypt

[storage]
filesystem_folder = /home/hatch/workspace/radicale-lab-work/storage

[rights]
type = owner_only

[web]
type = internal
```

Line by line:

- `[server] hosts` — where to listen.
  `127.0.0.1` is this machine only: the
  lab's choice, and the right one until
  you have read chapter 7 about putting
  the server in front of other machines.
  Port **5232** is Radicale's customary
  port.
- `[auth] type = htpasswd` — check
  passwords against a password file.
- `htpasswd_filename` — where that file
  is (chapter 5 creates it).
- `htpasswd_encryption = bcrypt` — the
  hashes in the file are bcrypt hashes.
- `[storage] filesystem_folder` — the
  folder of chapter 2. Everything
  Radicale serves lives under it.
- `[rights] type = owner_only` — the
  built-in rule: each user sees only
  their own collections. Chapter 6
  replaces it with a file of rules.
- `[web] type = internal` — turn on the
  small built-in web page for managing
  collections.

The paths are this lab's; on your
machine they point wherever you put
the server's folder. Nothing else in
the file is hidden: if a line is not
here, Radicale is using a documented
default.

## Step 3: first run

```bash
./venv/bin/radicale --config config
```

It stays in the foreground, logging
each request — leave it running and
use a second terminal for the checks.
Three answers tell you it is alive
(transcript t01):

```bash
curl -s -o /dev/null -w '%{http_code}\n' http://127.0.0.1:5232/.web/
# 200 — the built-in web page answers

curl -s http://127.0.0.1:5232/
# Redirected to /.web

curl -s -o /dev/null -w '%{http_code}\n' \
  -u alice:wrong-password http://127.0.0.1:5232/
# 401 — a password Radicale does not
# know is refused (alice has no account
# yet; chapter 5 fixes that)
```

That 401 is a good sign, not a failure:
the server is already checking. The log
says the same in words — "Login
verification failed for user: 'alice'".

**Check:** you are done when the web
page returns 200 and a made-up password
returns 401. Next chapter gives alice
a real password and something to
protect with it.
