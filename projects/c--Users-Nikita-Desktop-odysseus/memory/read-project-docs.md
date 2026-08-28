---
name: read-project-docs
description: "At the start of any odysseus session, read docs/PROJECT.md and docs/DECISIONS.md first"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 55f55c86-6c1c-4e77-b9a8-b9a9a370a264
---

В начале работы над проектом **odysseus** сначала прочитай `docs/PROJECT.md`
(живое состояние: стек, структура, договорённости, статус, открытые флажки) и
`docs/DECISIONS.md` (журнал решений с обоснованиями). По ходу работы — обновляй их.

**Why:** Володя хочет, чтобы контекст не терялся между сессиями и мы не ходили
по кругу через несколько дней. `AGENTS.md` тоже указывает на эти файлы.

**How to apply:** перед изменениями кода открой оба файла; после значимого шага
(новое решение, смена статуса, закрытый флажок) — допиши их. См. [[odysseus-overview]].
