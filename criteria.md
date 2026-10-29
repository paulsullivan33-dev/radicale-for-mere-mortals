# Writing criteria for this guide

Every "for Mere Mortals" guide is written to the same nine-point
standard, so a reader who trusts one can trust the rest. Here it
is, with the verification notes for this particular guide below.

1. **Fix it first.** Reader outcomes lead, not action items: each
   chapter opens by naming the outcome it delivers, shows a check
   the reader can run to confirm they got it, and a reasonable
   reader finishes able to state what was done, what it means,
   and what to try next.
2. **Plain language.** Short sentences, everyday vocabulary,
   technical terms defined on first use. No slang, and no
   idioms that only make sense in one country — the reader may
   be translating as they go.
3. **No dead ends.** A guide never tells the reader to do
   something it cannot show them how to verify. Every claim a
   reader relies on — an install, a command, an output — is
   something this guide demonstrates or clearly labels
   documented-only below.
4. **Live-verified where it matters.** Procedures are run for
   real in the guide's lab wherever the build machine allows it,
   and the transcripts kept in the repository are the evidence.
   What could not be run is listed honestly below, never
   presented as if it had been.
5. **General audiences.** Nothing in this guide — examples,
   fixtures, names, addresses — is modelled on any particular
   reader or author's real life, network, or organisations.
   The lab's users are invented (alice, bob), its club and its
   contact are fictional, and its passwords are throwaways.
6. **Standalone.** A reader needs this guide, its lab, and
   nothing else. Where another guide would genuinely help, it
   is pointed to once, by name, and the chapter still completes
   without it.
7. **Honest about limits.** Chapter 11 names what the tool
   cannot do: small scale by design, no push notifications of
   its own, no HTTPS of its own, a thin invitation layer, a web
   page that is an admin tool rather than a calendar, and a
   read-write share with no undo and no audit trail.
8. **The ethics rule is in the text, early.** Calendars and
   contacts are among the most personal data a person has —
   where they will be, and who matters to them. Chapter 1
   states the guide's line plainly: this data belongs on
   machines its owner controls, the fence between users is
   tested before any sharing is trusted, and every widening of
   access in chapter 6 is a deliberate, named rule — never a
   default. The lab demonstrates the principle on invented
   people only.
9. **Builds clean.** The site builds with MkDocs in strict mode
   with no warnings, and a fresh reader following the chapters
   in order reaches the stated outcome of each.

## Verification notes for this guide

The build machine is a shared Ubuntu 24.04 sandbox (Python
3.12.3). Radicale **3.9.0** was installed from PyPI into a
virtual environment inside the workspace (`pip install
"radicale[bcrypt]"`, which also brought libpass and bcrypt
5.0.0) and run unmodified on 127.0.0.1:5232 with the lab
configuration kept at `radicale-lab/lab-config.ini`. All
protocol work was driven with curl 8.5.0, speaking CalDAV and
CardDAV directly; the eight transcripts are in
`radicale-lab/transcripts/` (t00–t07), and the lab's sample
event, contact, config and rights file are in `radicale-lab/`.

Run for real, with transcripts as evidence:

- Install and version: Radicale 3.9.0 in a venv, dependency
  list recorded (t00).
- First run: root path redirecting to the built-in web page,
  web page answering 200, a wrong password refused with 401
  and the refusal written to the server log in words (t01).
- Principal discovery: PROPFIND on `/alice/` returning the
  calendar-home and addressbook-home; `MKCALENDAR` creating
  the "Riverside Camera Club" calendar (201), with the
  collection appearing on disk as a folder plus
  `.Radicale.props` (t02).
- Event round trip: PUT an iCalendar event (201), GET it
  back, and a CalDAV `REPORT` calendar-query with a November
  2026 time range returning exactly that event; the on-disk
  file listing of the calendar folder (t03).
- Address book: MKCOL with the CardDAV addressbook resource
  type (201), PUT a vCard contact (201), GET it back, and a
  CardDAV `REPORT` addressbook-query returning it (t04).
- The default fence (`rights type = owner_only`): the second
  user, bob, refused with 403 on reading alice's event,
  listing her principal, reading her contact, and writing
  into her calendar — while his own principal answered 207
  and an unauthenticated request answered 401 (t05).
- Sharing (`rights type = from_file`, rules at
  `radicale-lab/lab-rights`): an owner rule restoring full
  access to each user's own principal, plus a read-only rule
  (`permissions: Rr`) on the club calendar for bob. After a
  restart: bob read the shared event (200) and listed the
  calendar (207), was refused on PUT and DELETE (403, 403),
  stayed refused on the unshared address book (403), the
  owner added a second event (201), and bob could read that
  new event at once (200) (t06).
- Storage and backup: the stored event file `cat`-ed from
  disk and matching what was sent; the whole storage folder
  archived with `tar` (2,017 bytes), unpacked into a fresh
  folder, and both event summaries read back from the archive
  with no server involved (t07).

Documented only — described from the project's and the
client apps' documentation, not run, and the text does not
claim otherwise:

- Real client applications (Thunderbird, Apple Calendar and
  Contacts, DAVx5): the lab has no graphical session, so no
  app was clicked through. What is verified is the server
  side of every conversation those apps have (discovery,
  auth answers, collection access); the in-app steps are
  characterised from the apps' documentation.
- Using the built-in web interface beyond its answering 200:
  creating and editing collections by clicking, in a browser.
- Listening beyond 127.0.0.1 and serving other machines,
  including the reverse-proxy/HTTPS deployment shape and
  well-known discovery redirects on a real domain.
- Meeting invitations, scheduling and free/busy workflows,
  and push notification behaviour on phones.
- Authentication backends other than htpasswd (for example
  LDAP or remote-user), and hooks that run commands on
  changes.
- The read-write share in recipe 3: it is the same rights
  mechanism verified read-only in t06, with wider permission
  letters, and the recipe says so.
- The neighbour tools of chapter 10 (Nextcloud, DAViCal,
  Baïkal, the big clouds) — characterised, not run.

Fixtures are this guide's own and deliberately generic: two
invented users, a fictional camera club's calendar, one
invented contact at a fictional framing workshop, and
throwaway lab passwords stated in the text as such.
