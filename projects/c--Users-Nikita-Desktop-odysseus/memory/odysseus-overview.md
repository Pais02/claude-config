---
name: odysseus-overview
description: "What odysseus is and its core stack/conventions (finance app, Expo+Supabase)"
metadata: 
  node_type: memory
  type: project
  originSessionId: 55f55c86-6c1c-4e77-b9a8-b9a9a370a264
---

**odysseus** — мобильное приложение семейного/личного бюджета (дизайн «Bento
Budget»). Expo SDK 56 + React Native + Expo Router (корневой `app/`), NativeWind v4,
Supabase, TypeScript. Пакетный менеджер npm.

Ключевые инварианты: деньги — `bigint` в минорных единицах; мультивалюта только на
`transaction`/`subscription` (генерируемый `amount_base`); токены темы зеркалятся
из `src/theme/*.ts` в `tailwind.config.js`; Expo сильно изменился — перед кодом
читать доку v56.

Подробное и актуальное состояние держится в `docs/PROJECT.md` + `docs/DECISIONS.md`
(см. [[read-project-docs]]) — туда и писать, а не сюда.
