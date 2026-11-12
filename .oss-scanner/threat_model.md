# Threat model

## What this project does and where untrusted input enters
A Matrix homeserver (Python). Untrusted input: client API requests, federation events from other
homeservers, media uploads, presence/messages, and openID tokens. Federation is the largest
untrusted surface (remote servers send events that are deserialized and persisted).

## Components that matter most / least
- most: event deserialization/validation, the REST/JSON layer, media upload handling, and the
  crypto/signing code.
- less: administrative APIs, metrics.

## How to exercise it
Send crafted client requests and federation events; use the test suite and the sample data.
Focus on parsers that turn untrusted JSON/bytes into internal structures.

## How you rate severity
- remote code execution or auth bypass: critical.
- memory-safety or logic bugs that allow event injection / privilege escalation: high.
- denial of service via crafted events: medium.

## Anything to leave alone
- Do not report issues only reachable with a valid admin token as high severity.
