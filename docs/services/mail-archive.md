# Mail archive

> Self-developed, tamper-evident long-term archive for my personal mailboxes. It frees up space at the mail provider while keeping every message searchable and readable from any mail app.

| | |
|---|---|
| Type | **Self-developed** |
| Runtime | Docker Compose (portal, background worker, Dovecot, Roundcube, two nginx proxies) |
| Main technologies | Python, SQLite, Dovecot (IMAP), Roundcube (webmail), nginx |
| Status | In production |

## What it does

- Periodically synchronises configured provider mailboxes over IMAP (OAuth support for large providers is implemented)
- Imports historical mail from EML, MBOX and ZIP/Maildir exports
- Stores every message **immutably** together with its SHA-256 checksum and provenance
- Deduplicates logically identical messages (MIME-based) while keeping every distinct original variant
- Serves the archive **read-only** via IMAP (for any mail client) and webmail, separately per archive account
- Optionally deletes messages at the provider after a configurable age, but only after their safe archival has been proven

## Architecture

```mermaid
flowchart LR
    Prov["Mail providers<br/>(IMAP)"] -->|"sync"| W["Background worker"]
    Imp["Mail exports<br/>EML / MBOX / ZIP"] --> Portal["Admin portal"]
    Portal --> W
    W --> Store[("Immutable originals<br/>+ SQLite metadata<br/>+ HMAC-chained audit log")]
    Store -->|"read-only mount"| Dov["Dovecot IMAP"]
    Store -->|"read-only mount"| RC["Roundcube"]
    Client["Mail apps / browser<br/>(VPN mesh)"] --> TLS["TLS proxy"] --> Dov & RC
```

## Safe deletion at the provider

Deleting mail at the source is the dangerous part, so it is gated by several independent conditions. A message is only removed at the provider if **all** of the following hold:

```mermaid
flowchart TB
    A["Deletion enabled on host<br/>AND for this provider"] --> B["Complete, successful<br/>source synchronisation"]
    B --> C["Message archived and<br/>integrity-checked"]
    C --> D["Provider message re-verified<br/>right before deletion"]
    D --> E["Signed restore proof for its exact<br/>original bytes, max. 48 h old"]
    E --> F["Correct archive + backup disk<br/>and existing backup snapshot verified"]
    F --> G["Targeted deletion of exactly<br/>that message (UID EXPUNGE)"]
```

The restore proof comes from a separate backup-check service that restores the message from the backup into a scratch location, compares checksums and signs the result. Its signing key is not available to the portal or the worker, so a compromised web component cannot forge a proof.

## Hardening

- All containers run as unprivileged users with read-only root filesystems, no Linux capabilities and `no-new-privileges`
- No Docker socket in the stack
- IMAP and webmail containers have no internet access
- Admin login is separate from archive accounts; every archive account has its own data area and credentials
- Provider credentials are encrypted at rest
- No SMTP: the archive cannot be used to send mail

## Honest scope

This is a private, tamper-evident archive with tested backups. It is not a certified WORM or legal-compliance archive.
