---
name: project-trading-journal
description: "Trading Journal for Funded Traders — scope, decisions, and current build status"
metadata: 
  node_type: memory
  type: project
  originSessionId: beb7e059-9774-416f-a709-c3aae30c9518
  modified: 2026-08-21T13:40:53.210Z
---

Trading Journal — дневник сделок для funded-трейдеров с live rule-engine (daily loss limit, trailing/static drawdown, consistency rule) поверх Tradovate-синка. Дифференциатор от My Funded Book и аналогов: не постфактум-статистика, а расчёт "сколько ещё можно потерять сегодня/всего" в реальном времени под правила конкретной prop-фирмы.

Стек: Expo Router (React Native + web, один код на оба таргета) + Supabase (Postgres/Auth/Edge Functions/Storage) + Stripe (оплата на сайте, не в Telegram — обходит обязательный Telegram Stars для цифровых товаров) + Tradovate API для синка сделок.

**Why продукт задуман сразу как SaaS для других трейдеров** (не личный инструмент) — пользователь явно подтвердил это при старте (2026-08-21), поэтому RLS/multi-tenant заложены с первого дня, а не добавлены позже.

**How to apply:** любые архитектурные решения (схема БД, auth, биллинг) оценивать с точки зрения multi-tenant SaaS, а не single-user скрипта.

## Статус на 2026-08-21 (после первой сессии, недели 1-3 roadmap)

Сделано:
- Git-репозиторий локально инициализирован (без remote)
- Expo Router каркас (`src/app/`, не `app/` на корне — так сконфигурирован дефолтный шаблон `create-expo-app`, оставили как есть)
- Auth экраны (sign-in/sign-up), accounts CRUD с выбором firm preset
- Схема БД расширена таблицей `profiles` (Stripe-поля) поверх исходного `schema.sql`, накатана как первая Supabase-миграция
- Сид 4 firm presets (Apex/TopStep/FTMO/MFF) — структурные поля (drawdown_type и т.п.) заполнены, числовые лимиты (*_abs/*_pct) намеренно `null`, т.к. точные цифры меняются по фирмам/типам аккаунта и не были верифицированы
- Rule engine (`calculateLiveStatus`) с юнит-тестами на все 3 типа drawdown + consistency rule — все проходят
- Tradovate client + fakeClient + sync — протестировано только на fakeClient, реальных API-креды пока нет

Не сделано:
- Реальный Supabase cloud-проект (пользователь создаёт сам через дашборд — я не могу создать облачный аккаунт за него), после чего `supabase link` + `supabase db push`
- Tradovate demo/live API-креды (появятся когда пользователь заведёт demo-аккаунт)
- Живой прогон tradovate-sync и daily-snapshot (сейчас заготовки с TODO)
- Dashboard/Trades экраны — сейчас заглушки, реальная привязка к данным — неделя 4 по roadmap
- Stripe checkout/webhook

Полный план первой сессии: см. build-гайд [[user-stack-multiproject]] для паттернов, перенесённых из прошлых проектов.
