# Backing Up

By the end of this chapter the whole
server — every calendar, every
contact, every user — has been backed
up into one 2 KB archive, and the
restore has been rehearsed: the
archive unpacked somewhere else and
the events read straight out of it.
This is the chapter the storage design
(chapter 2) was promising.

## What a backup has to cover

Three folders and files, all named in
the config:

1. **The storage folder** — the
   calendars and contacts themselves.
2. **The password file** — without it,
   the data restores but nobody can
   log in as its owner.
3. **The config and rights files** —
   small, and miserable to reconstruct
   from memory (chapter 6's rules
   especially).

That is the complete list. There is no
database to dump, no export format to
convert through, because the storage
*is* the data in its native format.

## The lab's backup, for real

With the server stopped (a quiet
filesystem copies cleanly; Radicale
also tolerates being copied live, but
stopped is the honest rehearsal),
transcript t07 ran:

```bash
tar czf backup-test.tar.gz storage
# exit 0 — and the archive is 2,017 bytes
```

Two kilobytes, because the lab's whole
world is two events and a contact —
your real one will be bigger and still
trivially small; calendars are text.
The password file and configs are
one more `tar` line each beside it.

The restore rehearsal unpacked the
archive into a fresh folder and read
the events without any server at all:

```bash
tar xzf backup-test.tar.gz -C restore-check
grep -h SUMMARY restore-check/storage/collection-root/alice/club/*.ics
# SUMMARY:Darkroom night
# SUMMARY:Riverside Camera Club photo walk
```

A real restore is that, plus pointing
a Radicale install's
`filesystem_folder` at the unpacked
tree (and restoring the password
file), and starting it. Nothing is
imported or converted — the files were
never anything but the data.

## What to fold this into

This guide's backup is deliberately
the small, manual core. The standing
advice is to fold the three items
above into the backup routine you
already run — the series' Backups
guide (linked from the home page)
covers schedules, retention and
off-site copies with tools like
restic, and a Radicale storage folder
is one of the easiest things such a
routine will ever meet: small, plain,
and changed one file at a time.

One trap to avoid: do not back up
*only* the `.ics`/`.vcf` files and
skip the `.Radicale.props` files.
The props files carry each
collection's name and kind; without
them the events survive but the
calendars arrive unnamed and untyped.
Back up the folder, whole, and the
question never arises.

**Check:** your backup is real when
you have unpacked it somewhere new
and read an event out of it — the
rehearsal above, not the archive's
existence, is the evidence.
