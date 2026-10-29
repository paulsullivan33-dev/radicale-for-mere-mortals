# Troubleshooting

Start every diagnosis the same way:
the server's log. Radicale logs each
request and each refusal in words —
"Login verification failed for user:
'alice'", and the status it answered.
Half the entries below are the log
already telling you the answer.

## The client says the password is wrong — 401

Check with curl first (chapter 7's
test). A 401 with a password you
believe means one of: the user is not
in the password file (a login for a
name the file lacks fails exactly like
a wrong password — the lab met this in
t01 before alice existed), the
password was changed and the client
holds the old one, or the config's
`htpasswd_encryption` does not match
how the file's hashes were made (the
lab's file is bcrypt and the config
says so; a mismatch here fails every
login while looking like user error).

## A user gets 403 everywhere — even their own calendar

Almost always the rights file. If you
switched to `type = from_file`, the
owner rule from chapter 6 must be in
it; without it, nobody is granted
their own principal and every request
is refused. Read your rights file for
the `[owner-full-access]` rule before
touching anything else.

## A member cannot see the shared calendar — 403

Work chapter 6's list in order: is
there a rule naming *this* user? Does
its `collection:` pattern match the
calendar's real path (it is a pattern,
matched against paths like
`alice/club/...` — a typo silently
matches nothing)? Did the server
restart after the rule was added?
Then re-run the lab's test: PROPFIND
the collection as the member and read
the status. 207 means the share is
live and the client is stale — remove
and re-add the account in the app.

## The event is saved but not in the calendar

Usually the client cached, or the
event is outside the range the client
asked about. Ground truth is the disk:
look for the `.ics` file in the
storage folder (chapter 4). File
present means the server has it and
the disagreement is between client
and server about *asking* — a REPORT
for the event's month settles which.

## Two devices edited the same event

DAV clients guard writes with etags —
a version stamp the server checks
(the GETs in the lab's transcripts
show them). A client that was offline
can still force a conflict on some
setups; the symptom is one device's
edit silently winning. There is no
merge for calendar events anywhere;
the fix is social (one editor per
event) and the history cache sometimes
preserves the losing version as a
file you can re-add by hand.

## The server will not start

Read the first error, not the last:
an occupied port (another Radicale
still running — the lab killed its
first server before starting the
rights-file one), a config path that
does not exist, or a storage folder
the server's user cannot write. The
program is a single Python process;
if `radicale --version` works, the
install is fine and the problem is in
the config or the paths it names.
