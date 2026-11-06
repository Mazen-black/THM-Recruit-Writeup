# TryHackMe — Recruit

A hands-on penetration-testing write-up documenting the compromise of the **Recruit** training environment, from initial reconnaissance to HR and administrator access.

<sub>Educational lab write-up · Web application security · Authorized environment only</sub>

## Overview

The assessment demonstrates how several weaknesses can be chained into a larger compromise:

**Information disclosure → Local File Inclusion (LFI) → HR credential exposure → authenticated SQL injection → administrator access**

The key lesson is that individually limited findings can become critical when they expose the next step in an attack path.

## Lab scope

- **Platform:** TryHackMe
- **Target:** Recruit
- **Services observed:** SSH (22), DNS (53), HTTP (80)
- **Tools:** Nmap, Gobuster, curl, browser-based testing
- **Focus areas:** Web reconnaissance, directory listing, LFI, credential exposure, UNION-based SQL injection, remediation

All testing described here was performed against the intentionally vulnerable training target in an authorized lab environment.

## Assessment walkthrough

| Stage | What was assessed | Outcome |
|---|---|---|
| 1. Reconnaissance | Full TCP port scan and web directory enumeration | Identified the web application and exposed mail directory |
| 2. Information disclosure | Public directory listing and exposed mail log | Discovered internal deployment information that guided further testing |
| 3. LFI | File-fetching endpoint and local file access | Read application configuration and exposed the HR credential |
| 4. HR access | Authentication using the recovered lab credential | Obtained HR-level access |
| 5. SQL injection | Candidate search input and UNION-based testing | Confirmed a four-column query and enumerated database information |
| 6. Admin access | Retrieved administrator credentials from the lab database | Reached the administrator dashboard |

## Key security findings

- **Information disclosure:** Directory listing exposed an internal mail log.
- **Local File Inclusion / unsafe file retrieval:** User-controlled file input allowed access to a local application configuration file.
- **Credential exposure:** An application password was stored in a readable configuration file.
- **SQL injection:** An authenticated search function accepted injectable input, enabling database enumeration and credential extraction.
- **Chained impact:** The combined weaknesses escalated access from unauthenticated browsing to administrator-level application access.

## Defensive takeaways

- Disable directory listing and prevent public access to logs, backups, and deployment artifacts.
- Do not let user input determine filesystem paths. Use strict server-side allowlists or opaque file IDs mapped to approved files.
- Keep credentials outside source files and repositories; use environment-based secret management and rotate exposed credentials.
- Use parameterized queries / prepared statements for every database operation.
- Apply least privilege to application database accounts.
- Validate fixes with regression tests and verify that the full attack chain is no longer possible.

## Evidence

The detailed walkthrough includes screenshots for reconnaissance, directory discovery, the exposed mail log, configuration-file access, HR access, SQL injection testing, and administrator access.

See the complete, step-by-step report here:

**[Read Recruit-Writeup.md](Recruit-Writeup.md)**

Screenshots are stored in the [images](images/) directory.

## Skills demonstrated

Nmap · Gobuster · HTTP assessment · Linux · LFI analysis · SQL injection fundamentals · Database enumeration · Evidence collection · Security documentation · Remediation analysis

## Responsible use

This repository is intended for learning and portfolio review. The techniques and evidence relate to an authorized TryHackMe training environment only. Do not test these techniques against systems without explicit permission.

---

**Author:** Mazen Taha  
[GitHub profile](https://github.com/Mazen-black) · [LinkedIn](https://www.linkedin.com/in/mazen-taha-5a4608338/)

*Learning through hands-on labs, documenting findings, and focusing on practical security lessons.*
