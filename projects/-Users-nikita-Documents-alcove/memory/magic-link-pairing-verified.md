---
name: magic-link-pairing-verified
description: "Alcove pairing by email works end to end; what had to be fixed, and the Brevo click-tracking wrinkle"
metadata:
  type: project
---

Verified 2026-09-03 with a real email to the user's own inbox. The app sent the
link, the link returned through `alcove://auth-callback` with a PKCE code, the
session was exchanged, and the household was created under the display name the
app had written to disk before leaving for the inbox. Invite code minted. No
part of that path is stubbed.

**Two things had to be fixed to get there:**

1. `uri_allow_list` was empty — `alcove://auth-callback` was added via the
   Management API. Without it links land on the Site URL.
2. The built-in mailer is capped at **two emails an hour** and
   `rate_limit_email_sent` cannot be raised until custom SMTP exists. Brevo SMTP
   is now configured on the project (credentials also in `.env` as
   `BREVO_SMTP_*`), and the limit is 100/hour.

**Wrinkle worth remembering:** Brevo rewrites links for click tracking, so a
real email's link goes tracker -> Supabase `verify` -> app. `open-real-link.sh`
follows the chain. Anything that follows links in transit can spend a one-shot
token this way — an early attempt died exactly like that. Turning click
tracking off for transactional mail would remove the risk and a hop.

**Correction kept on purpose:** an earlier warning blamed the empty allow-list
for links pointing at localhost. That was a script bug —
`admin/generate_link` takes `redirect_to` at the top level and ignores it inside
`options`. The admin path never consults the allow-list; the client path does.

**Environment gotcha:** this network's router resolver drops DNS intermittently
("Resolving timed out"). `open-real-link.sh` falls back to 8.8.8.8 and pins the
address so a one-shot link is not wasted on a name lookup.

**Still not covered:** a second person redeeming the invite — that needs a
second device. See [[project-overview]] and [[mac-machine-setup]].
