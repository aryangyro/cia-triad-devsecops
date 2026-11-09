# CIA Triad in DevSecOps

A plain-English guide to the CIA triad — Confidentiality, Integrity,
Availability — and how it actually works inside DevSecOps. Written for
someone who is new to this: no assumed knowledge, one idea at a time,
with a real example for each part.

If you only read one section, read
[The CIA triad in one paragraph](#the-cia-triad-in-one-paragraph).
If you want the short version of my own take, skip to
[My write-up (summary)](#my-write-up-summary) and
[My reply to Mihirkumar (verbatim)](#my-reply-to-mihirkumar-verbatim).

## Contents

- [What this repo is](#what-this-repo-is)
- [1. What is DevSecOps?](#1-what-is-devsecops)
- [2. The CIA triad in one paragraph](#the-cia-triad-in-one-paragraph)
- [3. Confidentiality — keep secrets secret](#3-confidentiality--keep-secrets-secret)
- [4. Integrity — nothing gets changed behind your back](#4-integrity--nothing-gets-changed-behind-your-back)
- [5. Availability — it works when people need it](#5-availability--it-works-when-people-need-it)
- [6. How the three fit into a real pipeline](#6-how-the-three-fit-into-a-real-pipeline)
- [7. The tension between the three — and how to handle it](#7-the-tension-between-the-three--and-how-to-handle-it)
- [8. My write-up (summary)](#my-write-up-summary)
- [9. My reply to Mihirkumar (verbatim)](#my-reply-to-mihirkumar-verbatim)
- [10. What I took away from this](#10-what-i-took-away-from-this)
- [A beginner's checklist](#a-beginners-checklist)
- [Glossary](#glossary)
- [Further reading](#further-reading)

## What this repo is

This is a personal write-up about a simple question:

> Evaluate the role of the CIA triad (Confidentiality, Integrity,
> Availability) in DevSecOps practices. How can each component be
> ensured during software development?

That question sounds academic, but the problem behind it is very
practical. Every app you use — a banking app, a notes app, a shop,
a hospital system — has to do three things at once:

1. Keep private things private.
2. Make sure nothing has been secretly changed.
3. Actually be there when someone needs it.

When one of those three breaks, people notice fast. A leak, a
tampered file, or a site that is down at the wrong moment are all
the same kind of failure: the system did not keep its promises.

This repo explains each promise, shows how a team makes sure it
holds, and then gives my own write-up and reply on the topic, so
you can see the ideas applied instead of just defined.

## 1. What is DevSecOps?

### DevOps, in one sentence

DevOps is a way of building software where the people who write the
code and the people who run it work together, in small steps, all
the time — instead of writing everything for months and then throwing
it over a wall to be deployed.

In practice that means a pipeline: code is written, tested,
packaged and released automatically, many times, in small pieces.

### Then what does the "Sec" add?

DevSecOps is DevOps with security built into that pipeline from the
start, instead of added at the end.

The old way looked like this:

```
Write code -> Test it -> Ship it -> Someone checks security (too late)
```

The DevSecOps way looks like this:

```
Plan -> Write code -> Build -> Test -> Release -> Run and watch
         ^ security checks at every step ^
```

Security is not one big check at the end. It is a lot of small
checks, mostly automatic, at every stage. A scan runs when code is
committed. A test fails the build if a known-bad library is used.
A secret is never allowed into the code in the first place.

That matters because fixing a problem early is cheap, and fixing
it after release is expensive — and sometimes impossible, because
the damage is already done.

### The pipeline, stage by stage

| Stage | What happens | Where security fits in |
|---|---|---|
| Plan | Decide what to build | Think about what data is sensitive, who should see it, what could go wrong |
| Code | Developers write code | Code review, secure coding habits, no secrets in code |
| Build | Code is compiled and packaged | Signed builds, check the libraries you depend on |
| Test | Automated tests run | Security scans, check for known weaknesses |
| Release | The new version goes live | Final checks, deploy in a way you can roll back |
| Run and watch | Real users use it | Monitoring, alerts, logs, backups, patching |

The CIA triad is the test for every stage: at each step, ask
"does this keep things confidential, keep them un-tampered, and
keep the system available?"

## 2. The CIA triad in one paragraph

Think of the CIA triad as three promises a system makes to its users:

- **Confidentiality** — "Only the right people can see this."
- **Integrity** — "This has not been changed by someone who
  should not have changed it."
- **Availability** — "This works when you need it."

They are not three separate checklists. They are three sides of
the same thing: trust. A system you cannot trust with your data,
your files, or your time is not a secure system, however strong
any one of its three promises is on its own.

A quick example, using a simple online shop:

| Promise | What it means for the shop | What breaks it |
|---|---|---|
| Confidentiality | Only you can see your orders and payment details | A leak, or a staff account that can see everything |
| Integrity | The price you agreed to is the price you are charged; nobody swaps a product link for a fake one | A tampered page, a changed database record, a fake update |
| Availability | The shop loads and you can check out, even on a busy day | The site is down, or so slow it might as well be down |

Now the same three, for each part in detail.

## 3. Confidentiality — keep secrets secret

### What it is

Confidentiality means private information stays private. Only
people (and systems) who are supposed to see it can see it.

### A real example

Imagine a small clinic app that stores patient notes. A patient
expects that only their doctor and the clinic staff involved in
their care can read those notes — not other patients, not a
random employee, and not anyone who happens to find a copy of
the database.

If those notes leak, the harm is done the moment they are out.
You cannot un-leak something. That is why confidentiality is
usually the first thing people think of when they hear
"security", and why it needs to be designed in, not patched on.

### How it is ensured in a pipeline

**Encryption in transit and at rest.**
Data is scrambled so that anyone who intercepts it or steals a
copy cannot read it without the key. "In transit" means while
it travels over a network (use HTTPS/TLS, not plain HTTP).
"At rest" means while it is stored — in a database, on a disk,
in a backup. Encrypt both, not just one.

**Secrets out of source code.**
Passwords, API keys and tokens must never be written into code
or committed to a code repository. They leak that way constantly
— someone pushes code to a public repo, or a contractor gets
access, and the key is out. Use a **secrets manager** instead:
a tool that stores secrets in one safe place and hands them to
the app at run time, only when needed. If a secret does leak,
rotate it (replace it) immediately.

**Role-based access and least privilege.**
Not everyone needs to see everything. A support worker needs to
see a ticket, not a full medical record. **Role-based access
control (RBAC)** means you give people access based on their
role, and **least privilege** means you give them the smallest
amount of access that still lets them do their job. Review those
permissions regularly — access that nobody removes quietly
piles up over time.

**Treat logs and test data as real data.**
Two places where confidential data leaks quietly:

- **Logs:** an app that logs a full request can end up logging
  a password or a token. Log what you need to debug, and
  nothing sensitive.
- **Test data:** using a copy of the real customer database for
  testing means the test environment now holds real private
  data, usually with weaker protection. Use made-up data, or
  properly masked data, for tests.

### What good looks like

- Every connection uses encryption, and stored data is encrypted too.
- No password or key appears anywhere in the code.
- Each person and each service can only reach what they need.
- You could lose a laptop or a database backup and still not
  expose anyone's private information.

### What breaks it

- A password stored in plain text.
- A key committed to a public code repository.
- An account with far more access than its job needs.
- A log file, a screenshot or a test database that contains
  real customer data.

## 4. Integrity — nothing gets changed behind your back

### What it is

Integrity means data and code are what they are supposed to be.
Nobody has changed them without permission, and if someone did,
you can tell.

### A real example

You download an update for an app you use every day. How do you
know the file you got is the one the developers actually made,
and not a look-alike with something nasty added? You probably
do not check — you trust the process. Integrity is what makes
that trust justified: the file is signed, the download is
checked, and a tampered file gets rejected instead of installed.

The same idea applies inside a team. If someone changes a
price in a database, a line of code, or a configuration file,
the team needs to know who changed it, when, and whether it was
authorised. "It just changed" is not an acceptable answer.

### How it is ensured in a pipeline

**Signed commits and signed builds.**
A developer **signs** their code changes with a private key,
which proves the change really came from them. A **signed
build** does the same for the packaged app: it proves the
package was built from the code you think it was built from.
If the signature does not check out, the build is not trusted.

**Code review.**
Before code is merged, another person reads it. Review catches
mistakes, but it also catches things that look wrong on
purpose — a strange line that sends data somewhere odd, a
permission that got quietly widened. Review is not about
distrusting your team; it is about having a second pair of eyes
on every change, because one person misses things.

**Automated tests and security scans in CI/CD.**
Every time code is pushed, the pipeline runs tests automatically.
That includes security scans: checking for known weaknesses in
the code and in the libraries it depends on. A failing scan can
block the change from going further, which is the point — it is
much better to block a bad change than to ship it and find out
later.

**Verified dependencies.**
Modern apps are mostly other people's code — libraries and
packages. If one of those is compromised or out of date, your
app inherits the problem. Check what you depend on, keep it up
to date, and pin versions so a build today is the same as a
build tomorrow.

**Hashes and version history.**
A **hash** is a fingerprint of a file: change one byte and the
fingerprint changes completely. Storing a hash alongside a file
lets you check later whether the file is still the same. And
keeping everything in version control means every change has a
record — who, what, when — so a change can be traced and undone.

### What good looks like

- Every change is signed, reviewed and recorded.
- A tampered file, package or database record is detected,
  not silently accepted.
- You can rebuild the exact same version from the exact same
  code, and prove it.

### What breaks it

- Unsigned code that anyone could have written.
- A library that is never updated, with a known weakness.
- A database change with no record of who made it.
- A build that nobody can reproduce.

## 5. Availability — it works when people need it

### What it is

Availability means the system is up, reachable and usable when
people need it. A system that is perfectly locked down but down
half the time is not secure for its users — it is just useless.

People sometimes forget this one, because it does not feel like
"security". But an attacker does not have to steal anything to
cause real harm: taking a hospital system, a payment page or a
school portal offline at the wrong moment is damage on its own.

### A real example

An online shop runs a sale. Thousands of people arrive at once.
If the site crashes under the load, or a single server failing
takes the whole shop down, customers cannot buy anything — and
the business loses the day. Availability is the work that makes
sure that does not happen: more than one server, a plan for when
one fails, and a way to recover fast.

### How it is ensured in a pipeline

**Redundancy and failover.**
Do not have one of anything important. Run more than one server,
in more than one place if you can, so that when one fails the
others take over automatically. That automatic handover is
called **failover**. Test it — redundancy you have never tested
is a hope, not a plan.

**Monitoring and alerting.**
Watch the system while it runs: is it up, is it slow, are errors
rising, is disk space running out? Set up **alerts** so a human
finds out before users do. A dashboard nobody looks at is not
monitoring.

**Tested backups and recovery.**
Backups are only useful if you can restore from them, and
restoring is a skill you practise, not a button you press for
the first time in a crisis. Back up regularly, keep a copy
somewhere separate from the main system, and test a restore
from time to time. Know how long recovery takes — that is your
**recovery time**, and it should be a number you have measured,
not guessed.

**Security controls that do not block real users.**
This is the part people get wrong. A security check that blocks
legitimate users is an availability problem. A login system that
locks people out for no reason, a firewall rule that blocks a
whole office, a scan that stops every release — all of these make
the system less available. The controls should be strict about
attacks and invisible to normal use as far as possible.

**Capacity and rate limiting.**
Plan for busy days, not average days. And put limits on how
much any one user or system can ask for at once, so one abusive
or broken client cannot take the service down for everyone else.

### What good looks like

- One server failing is a non-event, not an outage.
- Someone is alerted the moment something starts going wrong.
- A backup has been restored in practice, not just in theory.
- Normal users barely notice the security controls.

### What breaks it

- A single point of failure: one server, one database, one
  network link.
- Backups that exist but have never been tested.
- Alerts that go to an inbox nobody reads.
- A security rule that locks out real users.

## 6. How the three fit into a real pipeline

Here is the same idea as a table you can walk through from left
to right, following one change from an idea to a running system.

| Pipeline stage | Confidentiality | Integrity | Availability |
|---|---|---|---|
| Plan | What data is sensitive? Who may see it? | What must never change without approval? | How much downtime can we accept? |
| Code | No secrets in code; use a secrets manager | Signed commits; code review | Write code that fails safely, not by crashing the whole service |
| Build | Build with the fewest permissions it needs | Signed build; check dependencies for known problems | Build so it can be rolled back quickly |
| Test | Test with fake or masked data, never real customer data | Automated tests and security scans; a failing scan blocks the release | Load-test before a busy day, not during it |
| Release | Encrypt everything in transit; rotate any exposed secret | Deploy only the signed, tested version | Release gradually; keep the old version ready to roll back to |
| Run and watch | Watch logs for leaks; limit who can read logs | Watch for unexpected changes; keep an audit trail | Monitor, alert, back up, and practise restoring |

Two things to notice:

1. **No stage is only about one promise.** Planning touches all
   three. So does testing. The triad is a lens you hold up to
   every stage, not a step you do once.
2. **Most of the work is automatic.** Encryption by default,
   signed builds, scans in the pipeline, alerts — the goal is
   that the safe thing happens without anyone having to remember
   it every time.

## 7. The tension between the three — and how to handle it

This is the part that makes the CIA triad interesting, and hard.
The three promises pull against each other.

- **More confidentiality can hurt availability.** Lock a system
  down too tightly and real users cannot get in. A password
  policy so strict that people write passwords on sticky notes
  has made things worse, not better.
- **More integrity checks can slow everything down.** A scan
  that blocks every release for a minor flag will be worked
  around — people will find a way to ship anyway, and then you
  have lost the check entirely.
- **More availability can weaken the other two.** Keeping
  everything running everywhere, with copies in many places,
  means more copies of sensitive data to protect, and more
  places where something can be changed.

A useful way to think about it: you are not trying to maximise
any one of the three. You are trying to find the balance where
the system is safe *enough* and still usable. "Safe enough"
depends on what is at stake — a banking app and a personal blog
do not need the same balance, and pretending they do is how
teams end up with either a fortress nobody can use, or an open
door.

### What helps in practice

**Make the safe thing the easy thing.**
The best controls are the ones nobody has to think about:
encryption switched on by default, signed builds, secrets
handled by a manager, scans running automatically. When the
secure path is also the easy path, people do not work around it.

**Fix the check, not the person.**
If a scan keeps flagging something minor and blocking deploys,
the answer is usually to tune the scan — or to fix the flagged
thing properly — not to tell people to hurry up and ignore it.
A check people ignore is worse than no check, because it looks
like protection while providing none.

**Measure it, where you can.**
Useful numbers to watch:

- How long does a release take, and how often is it blocked
  by a security check — and why?
- How often are people locked out, or asking for access they
  should already have?
- How long would recovery actually take, based on the last
  test, not on a guess?

There is no single number that tells you "you have gone too
far on security". But when delivery keeps slowing down, when
people start working around checks to ship, or when users
cannot get in — those are signals, and they are worth listening
to.

**Revisit the balance.**
The right balance changes. A small side project, a growing
startup and a hospital system need different answers, and the
same system needs a different answer as it grows. Revisit the
trade-offs on purpose, instead of letting them drift.

## 8. My write-up (summary)

> **Note on this section:** the exact wording of my original
> write-up is not saved in this repo, so this is a faithful
> summary of it — its angle, its points and its examples —
> not a word-for-word copy. The reply in the next section
> *is* word-for-word.

The question I was answering:

> Evaluate the role of the CIA triad (Confidentiality, Integrity,
> Availability) in DevSecOps practices. How can each component be
> ensured during software development?

My angle was different from just listing definitions. I framed
the CIA triad as **three promises a system has to keep at the
same time**, not three separate checklists — and DevSecOps as
building those promises into the pipeline instead of bolting
security on at the end.

The points I made, in order:

- **Confidentiality** is about keeping private things private:
  encryption when data travels and when it is stored, keeping
  secrets out of source code and in a secrets manager, access
  based on role and on least privilege, and treating logs and
  test data as if they were real production data.
- **Integrity** is about making sure nothing has been changed
  behind your back: signed commits, verified dependencies, code
  review by another person, and automated tests and security
  scans in the pipeline that can block a bad change.
- **Availability** is about the system working when people
  need it: redundancy and failover, monitoring and alerting,
  backups that have actually been tested, and security controls
  that do not block legitimate users.
- The interesting part is the **tension between the three**:
  favouring automated, mostly invisible controls — encryption
  by default, signed builds, automated scans — so the team
  does not have to think about security from scratch every
  single time.

## 9. My reply to Mihirkumar (verbatim)

Mihirkumar Manojkumar Prajapati had written on the same topic.
He framed the CIA triad as running through the whole software
development lifecycle, and as an ongoing goal rather than three
separate goals. He covered confidentiality (encryption, role-based
access, secrets management), integrity (signed commits, code
review, testing in the pipeline), and availability (testing,
monitoring, backups, redundancy) — and he raised the hard
balance between the three, with references to NIST SP 800-218
and NIST SP 800-53 Rev. 5.

This is my reply to him, word for word:

> Hi Mihirkumar, I liked how you framed CIA as an ongoing goal instead of three separate checklists, because that balance point you mentioned is where it actually gets hard in practice. I agree too much security can slow a team down, I have seen that with deploys getting blocked by a scan that flags something minor, and then people start finding ways around the check just to ship, which defeats the purpose.
>
> Your point about availability is interesting too, because a system that is locked down but down half the time is not really secure for the user either. I think the trick is making the controls mostly automatic, like signed builds and encryption by default, so the team does not have to think about them every time. Do you think there is a good way to measure when you have gone too far on security, or is it just something you feel when delivery starts slowing down?

The reply picks up his balance point, adds a concrete example
(a deploy blocked by a scan flagging something minor, and people
working around the check to ship), agrees that availability is
part of security rather than separate from it, and ends with a
question: is there a way to measure when security has gone too
far, or do you only feel it when delivery slows down?

## 10. What I took away from this

- The CIA triad is not a theory to memorise. It is a way to
  check any decision: does this keep things private, keep them
  un-tampered, and keep the system working?
- The three promises have to be kept together. A system that
  is confidential and has integrity but is never available is
  not doing its job. Neither is a system that is always up but
  leaks everything.
- The practical answer is usually automation. The controls
  that survive are the ones that run by themselves — encryption
  by default, signed builds, scans in the pipeline — because
  they do not depend on someone remembering to be careful.
- The hardest question is not "how do we add more security?"
  It is "how much is enough, and how do we know when we have
  gone too far?" That question does not have a formula. It has
  signals: delivery slowing down, people working around checks,
  users locked out. Pay attention to those.

## A beginner's checklist

If you are building something small and want to apply this,
start here:

**Confidentiality**
- [ ] All traffic uses HTTPS, and stored data is encrypted.
- [ ] No password, key or token is in the code or in version control.
- [ ] Secrets live in a secrets manager, not in a file or a chat message.
- [ ] Each user and service can only reach what they need.
- [ ] Logs and test data contain no real private information.

**Integrity**
- [ ] Code changes are signed and reviewed by another person.
- [ ] The pipeline runs automated tests and security scans on every change.
- [ ] Dependencies are checked and kept up to date.
- [ ] Builds are signed, so you know a package is the one you built.
- [ ] Every change is recorded, so it can be traced and undone.

**Availability**
- [ ] There is no single point of failure for anything important.
- [ ] Monitoring and alerts are set up, and someone actually sees them.
- [ ] Backups exist, and a restore has been tested.
- [ ] You know how long recovery takes, because you measured it.
- [ ] Security checks do not routinely block real users.

## Glossary

- **Availability** — the system is up and usable when people need it.
- **Backups** — copies of data kept so it can be restored after loss or damage.
- **CI/CD** — Continuous Integration / Continuous Delivery (or Deployment):
  code is merged, tested and released automatically, in small steps.
- **CIA triad** — Confidentiality, Integrity, Availability: the three
  core goals of information security.
- **Code review** — another developer reading a change before it is merged.
- **Confidentiality** — private information is only seen by those who
  should see it.
- **DevOps** — a way of working where development and operations work
  together, releasing software in small steps, often.
- **DevSecOps** — DevOps with security built into every stage of the
  pipeline, instead of checked only at the end.
- **Encryption** — scrambling data so it can only be read with the right key.
- **Failover** — automatically switching to a backup system when the main
  one fails.
- **Hash** — a fingerprint of a file or piece of data; any change to the
  data changes the hash.
- **Integrity** — data and code have not been changed without permission,
  and any change can be detected.
- **Least privilege** — giving a person or system only the smallest access
  needed to do their job.
- **Monitoring** — watching a running system for problems, slowness and errors.
- **RBAC** — Role-Based Access Control: access is granted based on a
  person's role.
- **Redundancy** — having more than one of something important, so one
  failure does not take the system down.
- **Role-based access** — see RBAC.
- **Secrets manager** — a tool that stores passwords, keys and tokens
  safely and hands them to an app only when needed.
- **Signed build** — a packaged app with a digital signature proving
  who built it and that it has not been changed since.
- **Signed commit** — a code change with a digital signature proving
  who made it.
- **TLS / HTTPS** — the encrypted protocol used to protect data travelling
  over a network, for example between a browser and a website.

## Further reading

- NIST SP 800-218, *Secure Software Development Framework (SSDF)* —
  a practical framework for secure development practices.
- NIST SP 800-53 Rev. 5, *Security and Privacy Controls for Federal
  Information Systems and Organizations* — the control catalogue
  behind many of the practices above.
- MITRE ATT&CK — a catalogue of how real attackers behave, useful
  for understanding what the controls above are defending against.

---

*Files in this repo: `README.md` (this guide) and `WORKLOG.md`
(what was done, and when).*
