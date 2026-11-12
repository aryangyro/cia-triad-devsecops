# Common Mistakes with the CIA Triad

Mistakes people make when they try to apply Confidentiality, Integrity
and Availability in real systems — and what each one costs. Each one
connects back to the examples in `README.md`.

## 1. Treating the three legs as equal

They are not. A hospital system that is down for an hour can cost
lives; a blog that is down for an hour costs readers. Which leg matters
most depends on the system. Decide the priority first, then design
around it — do not assume all three deserve the same effort.

## 2. Confusing encryption with access control

Encrypting a file does not decide *who* may read it. Encryption
protects data from outsiders; access control decides which insiders
may see it. You need both. A database that is encrypted but readable
by every employee is not confidential.

## 3. Backups that are never tested

A backup nobody has restored is a hope, not a plan. Test restores
regularly — a backup that fails when you need it is worse than no
backup at all, because it made you stop worrying. Availability depends
on restores working, not on backups existing.

## 4. Integrity checks that nobody reads

Checksums and audit logs only help if someone looks at them. An
integrity hash that is computed but never verified is decoration. The
check has to run automatically, and a failure has to alert a human —
otherwise tampering goes unnoticed for months.

## 5. Availability without thinking about abuse

Making a system always available also makes it always available to
attackers. Rate limiting, lockouts and throttling are availability
controls too: they keep the system usable for real users when someone
tries to flood it. Availability for everyone includes keeping
attackers from taking it all.

## 6. One control doing the job of three

A single password is asked to provide confidentiality (keep others
out), integrity (prove it was really you) and availability (let you
in when you need it). When one control carries all three legs, one
failure breaks everything. Layer controls — a password plus MFA, for
example — so no single failure takes the whole triad down.

## 7. Forgetting the human leg

The triad is usually drawn as a technical diagram, but most failures
are human: someone shares a password, clicks a phishing link, or
skips a step because it is inconvenient. A design that people work
around is a design that fails. If a control is too annoying to use,
people will bypass it — and then you have no control at all.

## How to use this file

Pick one mistake and find a real example of it — in a news story, in
a system you use, or in your own projects. Write down what broke, which
leg of the triad it broke, and what would have prevented it. One
concrete example teaches more than a chapter of theory.
