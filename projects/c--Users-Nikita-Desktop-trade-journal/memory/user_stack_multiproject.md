---
name: user-stack-multiproject
description: "User builds multiple SaaS side-projects with a consistent stack and reused patterns (Odysseus, OwlFlow, Trading Journal)"
metadata: 
  node_type: memory
  type: user
  originSessionId: beb7e059-9774-416f-a709-c3aae30c9518
  modified: 2026-08-21T13:41:03.065Z
---

Пользователь ведёт несколько side-project SaaS-продуктов и намеренно переиспользует стек/паттерны между ними, а не начинает каждый раз с нуля:

- **Стек**: Expo Router (React Native + web из одной кодовой базы) + Supabase (Postgres/Auth/Edge Functions/Storage) + Stripe для биллинга (сайт, не Telegram — обходит обязательный Telegram Stars для цифровых товаров)
- **Тестовый паттерн**: drop-in fake-клиент для внешних API в тестах — например, `FakeRemnawaveClient` в OwlFlow, тот же подход применён как `fakeClient.ts` в Trading Journal для Tradovate
- **Известные прошлые/параллельные проекты**: Odysseus (тот же Expo+Supabase стек, RLS-паттерн "как в Odysseus"), OwlFlow (fake-client тестовый паттерн)
- **Дисциплина по секретам**: усвоено из OwlFlow — ключи никогда не светить в чате/логах, при утечке считать скомпрометированным и ротировать немедленно

**How to apply:** при работе над любым из этих проектов (или новым похожим) — по умолчанию предлагать тот же стек и паттерны, если пользователь явно не просит иначе. Смотри [[project-trading-journal]] для текущего статуса конкретно Trading Journal.
