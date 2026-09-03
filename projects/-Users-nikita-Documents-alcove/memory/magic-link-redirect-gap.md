---
name: magic-link-redirect-gap
description: "Alcove magic links: the redirect works, the code exchange is unproven, and retries are capped at two emails an hour"
metadata:
  type: project
---

Tested 2026-09-03 against the live project (`drkwsdfkzawsyipfhujv`), with a
real email to the user's own inbox.

**Settled.** The project's `uri_allow_list` was empty; `alcove://auth-callback`
was added through the Management API. A link the app itself requested then
redirected to `alcove://auth-callback` rather than to the Site URL, so magic
links can reach the app on a device. The app handles a bad callback properly —
it showed "Email link is invalid or has expired" instead of failing silently.

**A correction worth keeping:** an earlier warning blamed the empty allow-list
for links pointing at localhost. That was a script bug —
`admin/generate_link` takes `redirect_to` at the top level and ignores it
inside `options`. The admin path never consults the allow-list at all; the
client path does.

**Still unproven:** exchanging a live code for a session. The token in the test
email was already spent when the link was followed, and had not timed out
(`mailer_otp_exp` is 3600s). Something consumed it in transit or on opening.

**Retries are rationed:** no custom SMTP (`smtp_host` null), so the built-in
sender is fixed at **two emails per hour**. `rate_limit_email_sent` cannot be
raised — the Management API refuses without SMTP credentials configured. Plan
around two attempts, and be careful with UI-test retry helpers: one blind
re-tap burned an attempt before this was understood.

**How to apply:** to finish this, either wait out the hour and try again, or
set up a custom SMTP provider first — which also fixes deliverability before
anyone else signs in. Scripts: `ios/scripts/send-real-link.sh` then
`open-real-link.sh`. See [[project-overview]] and [[mac-machine-setup]].
