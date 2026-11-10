# Exercises: the CIA triad in practice

Short exercises to go with the README. No setup needed — just think
through each one, then check the answer underneath.

## 1. Which leg broke?

Read each short story and name the leg of the triad that failed —
Confidentiality, Integrity, or Availability. (Sometimes more than one.)

**a)** A hospital's appointment system is fine, but a nurse prints a
patient list and leaves it on a bus seat.
> Answer: Confidentiality. The data itself was not changed and the
> system still worked, but private information reached people who
> should not have seen it.

**b)** A shop's website shows the right products, but someone changes
the prices in the database so everything costs 1 cent.
> Answer: Integrity. Nothing leaked and the site stayed up, but the
> data was changed without permission.

**c)** A bank's app is locked down tight and nothing leaks, but it is
down for a whole weekend and nobody can check their balance.
> Answer: Availability. The secrets and the data are fine, but the
> service is not there when people need it.

**d)** A ransomware attack encrypts a school's files and the attackers
also publish the students' addresses online.
> Answer: All three. Confidentiality (addresses published), Integrity
> (files changed), Availability (files unusable until restored).

## 2. Map a pipeline stage to a leg

For each pipeline stage, say which leg of the triad it protects most
and why — one sentence each.

- A linter that blocks secrets from being committed.
- A test that fails the build when a library has a known vulnerability.
- A backup that is restored and tested every month.
- A checksum check on every file a release downloads.

> Answers: the secret check is Confidentiality; the vulnerability test
> is mostly Integrity (the code you ship is what you meant to ship);
> the backup is Availability; the checksum is Integrity.

## 3. The trade-off call

A small team can only do one of these this week. Which would you pick,
and which leg does it protect?

- **A:** Turn on two-factor authentication for everyone who can deploy.
- **B:** Add a second server so the site stays up if one fails.
- **C:** Turn on checksums for every release file.

There is no single right answer — that is the point. What matters is
being able to say *which* leg you are leaving weaker and why. In most
small teams, A comes first, because a stolen deploy account can break
all three legs at once.
