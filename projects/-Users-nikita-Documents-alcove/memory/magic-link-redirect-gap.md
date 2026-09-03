---
name: magic-link-redirect-gap
description: "Alcove's magic-link return: what the redirect allow-list actually governs, and what is still unproven"
metadata:
  type: project
---

Investigated 2026-09-03 against the live project (ref `drkwsdfkzawsyipfhujv`).

**What was true:** the project's redirect allow-list (`uri_allow_list`) was
empty and Site URL was `http://localhost:3000`. `alcove://auth-callback` has
since been added, through the Management API.

**What was wrong in the earlier diagnosis:** the end-to-end script warned on
every run that links pointed at localhost, and that was blamed on the empty
allow-list. The real cause was the script itself — `admin/generate_link` takes
`redirect_to` at the **top level** of the body and silently ignores it inside
`options`. Sent correctly, minted links point at `alcove://auth-callback`, and
they still do with the allow-list emptied again: the admin path never consults
it.

**Still unproven:** whether the client path (`/otp`, what the app actually
calls) enforces the allow-list. It cannot be tested with throwaway addresses —
`/otp` rejects `.invalid` domains with `email_address_invalid` before looking
at the redirect at all. Settling it needs a magic link sent to a real inbox and
opened on a device, which also remains the only untested part of pairing.

**Also worth knowing:** the client signs in over PKCE, so the link exchange can
never be automated — the verifier exists only inside the client that requested
the link, and an admin-minted link fails with "Not a valid PKCE flow URL". The
end-to-end test installs a session directly and covers everything after it.

**How to apply:** do not describe email pairing as verified end to end; the
allow-list entry is correct but was not shown to be what was blocking anything.
See [[project-overview]] and [[mac-machine-setup]].
