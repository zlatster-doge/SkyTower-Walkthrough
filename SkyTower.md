# SkyTower — Walkthrough

**Disclaimer:** This document is a high-level summary of an authorized lab / CTF walk‑through. It intentionally omits exploit payloads, exact commands, and step‑by‑step instructions that could enable unauthorized access. Use only in isolated, authorized environments.

## Objective
Gain read access to the flag file located in the root filesystem.

## High-level summary
- Discovery: Locate target IP and verify host availability.
- Recon: Enumerated services and identified critical ports and services (SSH filtered, web server, HTTP proxy).
- Initial access: Web application login page exploited (SQL injection, details redacted) to obtain low‑privileged credentials for user `john`.
- Access via proxy: Direct SSH was filtered; an HTTP proxy on the target was used to route SSH traffic (via proxying tools).
- Post‑access enumeration: `john` had limited privileges and no sudo access; home directories revealed other users (`sara`, `william`).
- Further access & escalation: Additional user credentials were recovered (web flaw, details redacted). `sara` could run restricted commands that allowed reading files under root due to an unsafe configuration, enabling retrieval of the flag in /root.
- Final result: Flag retrieved from root (path and commands omitted).

## Key findings
- Exposed services: HTTP (web), HTTP proxy, filtered SSH.
- Application-level vulnerability: SQL injection on web login allowed credential recovery.
- Misconfiguration: Sudo policy allowed a restricted command path that could be abused to read root files.

## Recommended mitigations
- Fix input validation and eliminate SQL injection vulnerabilities in web apps.
- Harden proxy and SSH exposure; restrict or monitor proxy usage.
- Audit and tighten sudoers entries; avoid entries that permit arbitrary file reads or path traversal.
- Enforce least privilege for service accounts and rotate sensitive credentials.
- Maintain logging and alerting for unusual proxy/SSH activity.

## Repo guidance
- Keep this repo's documentation high‑level only. Do not include exploit payloads, raw credentials, or reproducible attack commands in public repositories.
- Store any test artifacts or proofs-of-concept in a private, access‑controlled location and sanitize them before sharing.

## References
- https://medium.com/@midhunkrishnanpu/skytower-vulnhub-walkthrough-24011ff2afc7
- https://www.infosecinstitute.com/resources/capture-the-flag/vulnhub-machines-walkthrough-series-skytower/
- https://medium.com/@Vishal17/proxychaining-using-linux-44d0ff00ba7c
- https://www.geeksforgeeks.org/linux-unix/how-to-setup-proxychains-in-linux-without-any-errors/
