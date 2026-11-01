# Euroton

Euroton is a self-hosted backend for the open-source Proton web clients. It answers the same API the clients already call, so Mail, Calendar & Account run on one Debian 13 box you control. It isn't made by, endorsed by or connected to Proton AG.

This repo holds the licensing audit & the project overview. The code lives in the repos below.

| Repo | What it is | Licence | State |
|---|---|---|---|
| [euroton-selfhost](https://github.com/Euroton/euroton-selfhost) | Ansible playbooks that install everything. Start here. | GPL-3.0 | Works |
| [euroton-server](https://github.com/Euroton/euroton-server) | The API backend: TypeScript, Fastify, Postgres | AGPL-3.0 | Works |
| [WebClients](https://github.com/Euroton/WebClients) | Fork of `ProtonMail/WebClients`, tag `euroton-0.1.0` | GPL-3.0 | Works |
| [euroton-godmode](https://github.com/Euroton/euroton-godmode) | Admin UI for the operator | AGPL-3.0 | Works |
| [openapi-spec](https://github.com/Euroton/openapi-spec) | OpenAPI 3.1 document extracted from the client source, 606 paths | GPL-3.0 | Works |
| [euroton-imap-bridge](https://github.com/Euroton/euroton-imap-bridge) | IMAP & SMTP adapter in Go | AGPL-3.0 | Skeleton, no working gateway |

Install instructions are in [euroton-selfhost/INSTALL.md](https://github.com/Euroton/euroton-selfhost/blob/main/INSTALL.md).

## What works

Sign-up, login, local mail between accounts on the same server & the admin UI run in the published install. Mail between two local accounts stays end-to-end encrypted, since the server stores the encrypted packages the client sends.

Calendar creates its default calendar, saves events through `/events/sync` & shows them after a reload. Reminders fire as browser notifications while the calendar tab is open: 15 minutes before a timed event by default (ADR 260). No server pushes them, so a closed tab gets nothing.

Outside mail is off by default. With it on, mail moved in both directions between 2 test servers on one network, with attachments, DKIM, threading & bounces (ADR 256), but no server has moved mail across the internet yet. Pass, VPN & Wallet are off by default because they 404-loop on most actions. Drive stays out; it needs a private package.

## Licences

Upstream is GPL-3.0, so the client fork & the code derived from it stay GPL-3.0. The two network services use AGPL-3.0, which means anyone hosting a modified copy has to publish their source. The server & the clients talk over HTTP as separate programs.

`licensing-matrix.md` has the per-package audit & the trademark inventory.

## Trademarks

"Proton" & "Proton Mail" are marks of Proton AG. The fork replaces Proton's logos with Euroton artwork. Remaining uses of the name describe where the code came from or what it's compatible with.
