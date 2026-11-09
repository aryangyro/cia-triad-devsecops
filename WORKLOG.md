# Work log — Nov 8, 2026

## What I did today
- Wrote up my take on the CIA triad in DevSecOps, about
  how the three principles actually work together in practice.
- Replied to a classmate who had posted on the same topic, picking up on
  his point about balancing security with delivery speed, with a concrete
  example and a closing question.

## Challenges
- The balance point is the hard part to write about honestly — too much
  security slows delivery, too little leaves users exposed, and there is
  no clean formula for where the line is.

---

# Work log — Nov 8, 2026 (evening): README expansion

## What I did today
- Expanded the README from a short write-up into a long, beginner-friendly
  guide, so someone new to the topic can understand what was made without
  any background knowledge.
- Added: a plain-English explanation of DevSecOps and the pipeline stages;
  the CIA triad explained as three promises, with a real example for each
  (a clinic app for confidentiality, a tampered download for integrity,
  a shop going down on a busy day for availability); how each promise is
  ensured in a pipeline (encryption in transit/at rest, secrets manager,
  role-based access and least privilege, signed commits and signed builds,
  code review, automated tests and security scans, verified dependencies,
  redundancy and failover, monitoring and alerting, tested backups and
  recovery); a stage-by-stage pipeline table; and a section on the tension
  and balance between the three.
- Added a beginner's checklist, a glossary, and further reading
  (NIST SP 800-218, NIST SP 800-53 Rev. 5, MITRE ATT&CK).
- Added the original write-up as a clearly labelled summary and the reply
  to Mihirkumar Manojkumar Prajapati in full, word for word.

## What I could and could not find
- The reply text was available verbatim and is quoted in full in the README.
- The exact wording of the original write-up was NOT found in any saved
  file or note I searched (memory notes, daily logs, and workspace files
  only record its prompt, its angle — CIA as three promises — and a
  summary of its points). So the README presents it as a faithful
  summary, labelled as such, and does not invent verbatim wording for it.

## Challenges
- Keeping the guide beginner-friendly without dumbing it down: every
  term needed a plain explanation the first time it appeared, and every
  control needed a reason, not just a name.
- Making sure the expanded README stays a plain personal write-up,
  with no college, course, or submission details in it.
