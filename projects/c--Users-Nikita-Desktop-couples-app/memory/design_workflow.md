---
name: design-workflow
description: User prefers designing screens in Claude Design canvas before writing any SwiftUI code
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 3895e366-585e-4793-bbf5-b75d9c3c3907
  modified: 2026-08-27T16:02:04.063Z
---

For this project, design each sprint's screens as wireframes in Claude Design (the `design` skill) before writing SwiftUI. Confirmed working well for Sprint 1 (Pairing + Wishlist + Events) — user reaction: "выглядит супер", no pushback on the approach or the choices made.

**Why:** Володя wanted to validate flow/UX cheaply before committing to Swift code, since rewriting SwiftUI views after a design change is more expensive than reworking an HTML wireframe. See [[project-overview]].

**How to apply:**
- Fidelity: wireframe/flow level, not pixel-perfect final visuals — structure, states (empty/filled), and screen-to-screen flow matter more than polish at this stage.
- Components: strictly native iOS HIG (TabView bottom bar, large-title NavigationStack, standard List/Form, sheet modals with Cancel/Title/Save header) — no custom navigation — so the translation to SwiftUI is close to 1:1.
- Aesthetic: when the user has no reference/preference, he's fine delegating it entirely ("придумай сам") — for this app he wants warm, private, intimate, minimalist, light/dark aware. Don't re-ask for style on later sprints unless he wants to change direction.
- Static wireframes connected by flow annotations (sticky notes describing transitions) are enough — no need for a tap-through clickable prototype unless he asks for one specifically.
- After a multi-artboard build, background-check the working `.dc.html` files for consistency (shared CSS tokens, no duplicate class names with different values, matching sample data across screens) before calling it done — this caught 3 real bugs on Sprint 1 (a dual-active tab state, a class name reused with two different sizes, a mismatched event title between the add-form and the list).
