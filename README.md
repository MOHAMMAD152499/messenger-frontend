<div align="center">

<img src="public/notification-icon-192.png" width="104" alt="Messenger logo" />

# Messenger Frontend

**A production-ready, fully responsive web messenger — the official frontend of [Squsre/messenger-backend](https://github.com/Squsre/messenger-backend)**

[![Next.js](https://img.shields.io/badge/Next.js-16-black?logo=next.js)](https://nextjs.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-strict-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-4-06B6D4?logo=tailwindcss&logoColor=white)](https://tailwindcss.com)
[![Zustand](https://img.shields.io/badge/State-Zustand-orange)](https://zustand.docs.pmnd.rs)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Backend](https://img.shields.io/badge/Backend-Squsre%2Fmessenger--backend-009688?logo=fastapi&logoColor=white)](https://github.com/Squsre/messenger-backend)

**🌐 مستندات فارسی: [README.fa.md](./README.fa.md)**

</div>

---

## ✨ Introduction

**Messenger Frontend** is a complete, polished chat application for the browser: realtime
direct and group messaging, media sharing, contacts, profiles, invitations and a full
settings suite — wrapped in a responsive three-column desktop shell and a mobile
single-view layout with bottom navigation.

It is developed **as the frontend of the [Messenger backend project](https://github.com/Squsre/messenger-backend)**
(FastAPI + PostgreSQL + Redis + MinIO). Every endpoint, WebSocket frame, validation rule
and limit in this app is mirrored directly from that backend's OpenAPI schema
(`openapi.json`), so the two projects form one coherent product.

> 🔗 **Related project** — Backend repository: **https://github.com/Squsre/messenger-backend**
> This repository is the web frontend of that project and requires it as its API server.

## 🚀 Features

**Realtime messaging**
- Single WebSocket connection with an authentication handshake (< 5 s), ping/pong heartbeat, stale-link detection and exponential-backoff reconnect
- `ready`-frame bootstrap: conversation reconcile + bounded list catch-up, so busy accounts see every chat after each (re)connect
- Optimistic message sending (`client_message_id`) with socket acknowledgement reconciliation and a REST fallback when the socket is down
- Delivered/read watermarks, status ticks (pending → sent → delivered → read), group read counters, unread badges in the sidebar, tab title and group lists
- Typing indicators, day separators, "Unread messages" divider, unread-badged scroll-to-bottom FAB

**Media**
- Drag & drop + picker uploads (10 MiB per file, up to 4 files: JPEG, PNG, GIF, WebP, PDF, TXT, CSV) with per-file progress and real cancellation
- Presigned-URL caching with early refresh, self-healing re-resolve and an authenticated `/content` blob fallback that survives dead links
- Image lightbox, document cards with download, per-attachment file-info dialog, attach-to-existing-message

**Groups**
- Public/private group creation, public group discovery with race-guarded search + paging and one-click join
- Invitations with accept/decline, outgoing invitation history with status chips and revoke
- Roles (owner/admin/member), promote/demote, remove, ownership transfer and leave — all with confirmation dialogs and 409-prevention guards
- Live member panel updates on WS join/leave/kick/role/ownership events, auto-pagination for 100+ member groups

**Auth & security**
- Login, register, email verification (with a pinned resend banner) and password reset
- Hardened JWT storage: tokens live in memory + an `auth_token` cookie (`SameSite=Lax`, `Secure` under HTTPS) — never in `localStorage`
- Cookie-based route guard (`proxy.ts`, Next 16 middleware convention), 401 / WS-4401 auto-logout with rehydration from the cookie on boot

**Contacts & profiles**
- Dedicated contacts page (add by username, search, remove, one-click chat, paging) plus add/remove in every profile panel
- Public profile drawer (`GET /users/{username}`): bio, joined date, last seen, visibility
- Blocking with a managed blocked-accounts list; privacy controls for profile visibility and last seen

**Settings & notifications**
- Profile editing, appearance (dark mode + 6 accent themes), privacy, password change, active-session revocation
- About tab with live backend diagnostics: API banner, liveness and readiness with per-dependency status
- Desktop notifications for direct and group messages (click-to-open, per-chat tag) and an unread `(N)` tab-title badge

**UX & international**
- Global `Ctrl/Cmd+K` command palette (chats, groups, contacts, actions), per-chat draft persistence, auto-resizing composer with emoji picker
- Full BiDi support (`dir="auto"`): Persian/Arabic and Latin messages align automatically; local Vazirmatn variable font covers Persian typography
- Fully responsive: three-column desktop (minimizable chat list), mobile single-view with a 5-entry bottom navigation bar, mobile-safe settings layout

## 🧱 Tech Stack

| Layer | Technology |
| ----- | ---------- |
| Framework | [Next.js 16](https://nextjs.org) (App Router, standalone output) |
| Language | TypeScript (strict) |
| UI | Tailwind CSS 4 · shadcn/ui · Radix UI · lucide-react · framer-motion |
| State | Zustand (+ immer, persist) · TanStack Query |
| Forms | react-hook-form + zod |
| Realtime | Native WebSocket with a custom frame contract (`src/types/ws.ts`) |
| Fonts | Vazirmatn (variable) · Rajdhani · Cinzel Decorative — all SIL OFL, bundled locally |

## 🏁 Getting Started

### Prerequisites

- **Node.js 20+** (npm / pnpm) or **Bun 1.2+**
- A running [Messenger backend](https://github.com/Squsre/messenger-backend) instance

### Installation

```bash
git clone https://github.com/MOHAMMAD152499/messenger-frontend.git
cd messenger-frontend

bun install        # or: npm install / pnpm install
cp .env.example .env.local   # then edit the values
```

### Development

```bash
bun run dev        # http://localhost:3000
```

### Production

```bash
bun run build      # next build + standalone bundle (static assets copied in)
bun run start      # serves .next/standalone/server.js
```

## 🔌 Connecting to the Backend

Configure the API target in `.env.local` (see [`.env.example`](.env.example)):

| Variable | Description |
| -------- | ----------- |
| `NEXT_PUBLIC_API_BASE_URL` | Absolute base URL of the Messenger API, e.g. `https://messenger.example.com`. When **unset**, the app runs in *gateway mode*: all requests go to relative `/api/v1/...` paths with an `XTransformPort` query parameter. |
| `NEXT_PUBLIC_MOCK_API_PORT` | Port of the mock/gateway service in gateway mode (default `3030`). |

The WebSocket endpoint is derived from the same base (`/api/v1/ws`). All REST and WS
contracts are defined by the backend's `openapi.json` — see the
[backend repository](https://github.com/Squsre/messenger-backend) for deployment and
API documentation.

## 📁 Project Structure

```text
src/
├── app/
│   ├── (auth)/            # login, register, verify-email, reset-password
│   ├── (main)/            # app shell + empty state, c/[id], g/[id], settings, contacts
│   ├── layout.tsx
│   └── globals.css
├── components/
│   ├── ui/                # shadcn/ui primitives
│   ├── layout/            # NavRail, BottomNav, ChatSidebar, InfoSidebar (profile/group panels)
│   ├── chat/              # ChatViewport, MessageFeed, MessageItem, media, ChatInput, EmojiPicker
│   ├── dialogs/           # NewChat, CreateGroup, Invitations, DiscoverGroups, CommandPalette
│   ├── settings/          # AboutPanel (API banner / health / readiness)
│   └── common/            # UserAvatar, StatusTick, EmptyState, providers
├── hooks/                 # use-media-url, use-mobile, use-typing, use-race-guard, use-unread-tab-title
├── lib/                   # api-client, validation (zod), constants, media-url-cache, notifications, draft-store
├── stores/                # auth, conversation, message, socket, contacts, blocks, ui (+ reset registry)
├── types/                 # api.ts (from openapi.json), ws.ts (frame contract)
└── proxy.ts               # cookie route guard (Next 16 middleware convention)
public/
├── fonts/                 # Vazirmatn, Rajdhani, Cinzel Decorative (SIL OFL)
└── logo.svg, notification-icon-*.png, robots.txt
```

## 📐 Backend Contract Notes

- Access tokens live 15 minutes; any HTTP `401` or WS close `4401/4403` purges the session and redirects to `/login?expired=1`.
- The WS auth frame is sent as the first frame within 5 s; server `ping` frames are answered immediately; the client also pings every 25 s and recycles links silent for > 70 s.
- Typing frames are throttled to one `true` per 1.5 s; read frames are throttled to 1 s and never repeat an older `last_message_id`.
- `delivered`/`read` events are watermarks: every message with `id <= last_message_id` is updated in one pass.
- Password policy: 12–128 characters with upper, lower, digit and special character; username policy: 3–32 chars, lowercase letters/digits/underscores.
- After every reconnect both list endpoints are refreshed so server-side unread counters resync; every collection endpoint pages with `limit`/`offset` and merges dedupe by id.

## 📚 Documentation

- [`docs/UNIMPLEMENTED_ENDPOINTS.md`](docs/UNIMPLEMENTED_ENDPOINTS.md) — full endpoint-coverage audit (pagination, search race guards, invitation history, uploads hardening)
- [`docs/AUDIT-v2.1.0-category-4-5.md`](docs/AUDIT-v2.1.0-category-4-5.md) · [`docs/AUDIT-v2.2.0-category-6.md`](docs/AUDIT-v2.2.0-category-6.md) — release audit notes

## 🤝 Contributing

Issues and pull requests are welcome. Please keep changes consistent with the backend
API contract and run `npm run lint` before submitting.

## 🔒 Security Notes

- Access tokens are never persisted in `localStorage`; they live in memory and an `httpOnly=false` but `SameSite=Lax` (+ `Secure` on HTTPS) cookie used only for boot rehydration.
- All configuration is provided through environment variables — no secrets are committed. Copy `.env.example` to `.env.local` for local values.

## 📄 License

Released under the [MIT License](LICENSE).
The bundled fonts (Vazirmatn, Rajdhani, Cinzel Decorative) are distributed under the
[SIL Open Font License](public/fonts/).

---

<div align="center">

**The frontend of [Squsre/messenger-backend](https://github.com/Squsre/messenger-backend)** · Built with Next.js

</div>
