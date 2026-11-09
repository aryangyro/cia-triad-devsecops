# CIA Triad in DevSecOps

A short write-up I did because I was curious about how Confidentiality,
Integrity and Availability actually hold together in DevSecOps, instead
of just being three words in a textbook.

## My take
I think of the CIA triad as three promises a system has to keep at the
same time, not three separate checklists, and DevSecOps as building those
promises into the pipeline instead of bolting security on at the end.

- Confidentiality: encryption in transit/at rest, secrets out of source
  code, a secrets manager, role-based/least-privilege access, treating
  logs and test data as production data.
- Integrity: signed commits, verified dependencies, code review, automated
  tests and security scans in CI/CD.
- Availability: redundancy/failover, monitoring/alerting, tested
  backups/recovery, and security controls that do not block legitimate users.

The interesting part is the tension between the three — favouring automated,
mostly invisible controls such as encryption by default, signed builds and
automated scans.

## Reply — the balance problem
I also replied to a classmate who framed CIA as an ongoing goal through
the whole SDLC. His closing point was the hard balance: too much security
slows a team down, ignoring availability leaves users exposed. My reply
picked up on that — deploys blocked by a scan flagging something minor,
people working around the check to ship — and asked whether there is a
good way to measure when security has gone too far, or whether you only
feel it when delivery slows down.
