<div dir="rtl">

<div align="center">

<img src="public/logo.svg" width="104" alt="لوگوی پیام‌رسان" />

# فرانت‌اند پیام‌رسان (Messenger Frontend)

**یک وب‌اپلیکیشن چت کامل، آمادهٔ انتشار و کاملاً ریسپانسیو — فرانت‌اند رسمی پروژهٔ [Squsre/messenger-backend](https://github.com/Squsre/messenger-backend)**

[![Next.js](https://img.shields.io/badge/Next.js-16-black?logo=next.js)](https://nextjs.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-strict-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-4-06B6D4?logo=tailwindcss&logoColor=white)](https://tailwindcss.com)
[![Zustand](https://img.shields.io/badge/State-Zustand-orange)](https://zustand.docs.pmnd.rs)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Backend](https://img.shields.io/badge/Backend-Squsre%2Fmessenger--backend-009688?logo=fastapi&logoColor=white)](https://github.com/Squsre/messenger-backend)

**🌐 English documentation: [README.md](./README.md)**

</div>

---

## ✨ معرفی

**فرانت‌اند پیام‌رسان** یک تجربهٔ چت کامل و حرفه‌ای برای مرورگر است: پیام‌رسانی لحظه‌ای
شخصی و گروهی، اشتراک‌گذاری رسانه، مخاطبین، پروفایل‌ها، دعوت‌نامه‌ها و یک مجموعهٔ تنظیمات
کامل — همه در قالب یک چیدمان ریسپانسیو؛ سه‌ستونه در دسکتاپ و تک‌نمایی همراه با نوار
ناوبری پایین در موبایل.

این پروژه به‌عنوان **فرانت‌اند رسمی پروژهٔ بکند [Messenger backend](https://github.com/Squsre/messenger-backend)**
(FastAPI + PostgreSQL + Redis + MinIO) توسعه یافته است. تمام endpointها، فریم‌های
WebSocket، قواعد اعتبارسنجی و محدودیت‌های این اپلیکیشن مستقیماً از اسکیمای OpenAPI آن
بکند (`openapi.json`) برداشته شده‌اند؛ به‌گونه‌ای که این دو پروژه یک محصول منسجم را تشکیل می‌دهند.

> 🔗 **پروژهٔ مرتبط** — مخزن بکند: **https://github.com/Squsre/messenger-backend**
> این مخزن، فرانت‌اند وبِ آن پروژه است و برای اجرا به API سرور آن نیاز دارد.

## 🚀 امکانات

**پیام‌رسانی لحظه‌ای**
- یک اتصال WebSocket با هندشیک احراز هویت (کمتر از ۵ ثانیه)، ضربان قلب ping/pong، تشخیص اتصال مرده و اتصال مجدد با backoff نمایی
- bootstrap فریم `ready`: همگام‌سازی مکالمه‌ها و تکمیل محدود لیست‌ها پس از هر (اتصال مجدد) تا کاربران پرمکالمه همهٔ چت‌ها را ببینند
- ارسال خوش‌بینانهٔ پیام (`client_message_id`) با تطبیق‌سازی از طریق ack سوکت و fallback به REST هنگام قطعی سوکت
- واترمارک‌های delivered/read، تیک وضعیت (در انتظار → ارسال → تحویل → خوانده‌شده)، شمارندهٔ خوانده‌شدن گروهی و badgeهای نامحت‌شده در سایدبار، عنوان تب و لیست گروه‌ها
- نشانگر «در حال تایپ»، جداکنندهٔ روزها، جداکنندهٔ «پیام‌های نامحت‌شده» و دکمهٔ شناور پرش به پایین با شمارندهٔ نامحت‌شده

**رسانه**
- آپلود با درگ‌ودراپ و انتخابگر (۱۰ مگابایت برای هر فایل، حداکثر ۴ فایل: JPEG، PNG، GIF، WebP، PDF، TXT، CSV) همراه با پیشرفت مجزا برای هر فایل و لغو واقعی آپلود
- کش presigned URL با تمدید زودهنگام، رفع‌خرابی خودکار و fallback احراز هویت‌شدهٔ `/content` برای لینک‌های ازکارافتاده
- لایت‌باکس تصاویر، کارت اسناد با دکمهٔ دانلود، دیالوگ اطلاعات فایل برای هر پیوست و امکان پیوست به پیام‌های قبلی

**گروه‌ها**
- ساخت گروه عمومی/خصوصی، جستجوی گروه‌های عمومی با ریس‌گارد و صفحه‌بندی و عضویت یک‌کلیکی
- دعوت‌نامه‌ها با پذیرش/رد، تاریخچهٔ دعوت‌نامه‌های خروجی با status chip و امکان لغو
- نقش‌ها (مالک/ادمین/عضو)، ارتقا/تنزل، حذف عضو، انتقال مالکیت و خروج از گروه — همه با دیالوگ تأیید و محافظ‌های ضد خطای 409
- به‌روزرسانی زندهٔ پنل اعضا با رویدادهای WS (join/leave/kick/role/ownership) و صفحه‌بندی خودکار گروه‌های بیش از ۱۰۰ عضو

**احراز هویت و امنیت**
- ورود، ثبت‌نام، تأیید ایمیل (با بنر ارسال مجدد) و بازنشانی رمز عبور
- ذخیره‌سازی امن JWT: توکن‌ها فقط در حافظه و کوکی `auth_token` (`SameSite=Lax` و `Secure` زیر HTTPS) نگه‌داری می‌شوند — هرگز در `localStorage`
- گارد مسیر مبتنی بر کوکی (`proxy.ts`)، خروج خودکار با 401 یا بستن سوکت 4401 و بازیابی نشست از کوکی هنگام بوت

**مخاطبین و پروفایل**
- صفحهٔ اختصاصی مخاطبین (افزودن با نام کاربری، جستجو، حذف، چت فوری و صفحه‌بندی) به‌همراه افزودن/حذف در همهٔ پنل‌های پروفایل
- پنل پروفایل عمومی (`GET /users/{username}`): بیو، تاریخ عضویت، آخرین بازدید و سطح نمایان‌بودن
- مسدودسازی با لیست قابل مدیریت و تنظیمات حریم خصوصی (سطح نمایش پروفایل و آخرین بازدید)

**تنظیمات و اعلان‌ها**
- ویرایش پروفایل، ظاهر (حالت تیره + ۶ تم رنگی)، حریم خصوصی، تغییر رمز و لغو نشست‌های فعال
- تب «درباره» با تشخیص سلامت زندهٔ بکند: بنر API، liveness و readiness با وضعیت تک‌به‌تک وابستگی‌ها
- اعلان‌های دسکتاپ برای پیام‌های شخصی و گروهی (کلیک برای باز شدن چت) و badge `(N)` در عنوان تب

**تجربهٔ کاربری و چندزبانه**
- پالت فرمان سراسری `Ctrl/Cmd+K`، ذخیرهٔ پیش‌نویس هر چت، ادیتور خودکارِ اندازه با انتخابگر ایموجی
- پشتیبانی کامل BiDi (`dir="auto"`): پیام‌های فارسی/عربی و لاتین به‌صورت خودکار راست‌چین/چپ‌چین می‌شوند؛ فونت متغیر وزیرمتن به‌صورت لوکال برای تایپوگرافی فارسی
- کاملاً ریسپانسیو: سه‌ستونه در دسکتاپ (با لیست چت قابل جمع‌شدن)، تک‌نمایی در موبایل با نوار ناوبری ۵ بخشی و چیدمان تنظیمات مناسب موبایل

## 🧱 پشتهٔ فناوری

| لایه | فناوری |
| ----- | ---------- |
| فریم‌ورک | [Next.js 16](https://nextjs.org) (App Router، خروجی standalone) |
| زبان | TypeScript (strict) |
| رابط کاربری | Tailwind CSS 4 · shadcn/ui · Radix UI · lucide-react · framer-motion |
| مدیریت state | Zustand (+ immer, persist) · TanStack Query |
| فرم‌ها | react-hook-form + zod |
| ارتباط لحظه‌ای | WebSocket بومی با قرارداد فریم سفارشی (`src/types/ws.ts`) |
| فونت‌ها | وزیرمتن (variable) · Rajdhani · Cinzel Decorative — همگی با مجوز SIL OFL و به‌صورت لوکال |

## 🏁 شروع سریع

### پیش‌نیازها

- **Node.js 20+** (npm / pnpm) یا **Bun 1.2+**
- یک نمونهٔ در حال اجرای [بکند پیام‌رسان](https://github.com/Squsre/messenger-backend)

### نصب

```bash
git clone https://github.com/MOHAMMAD152499/messenger-frontend.git
cd messenger-frontend

bun install        # یا: npm install / pnpm install
cp .env.example .env.local   # سپس مقادیر را ویرایش کنید
```

### محیط توسعه

```bash
bun run dev        # http://localhost:3000
```

### محیط تولید

```bash
bun run build      # next build + باندل standalone (کپی فایل‌های استاتیک)
bun run start      # اجرای .next/standalone/server.js
```

## 🔌 اتصال به بکند

هدف API را در فایل `.env.local` تنظیم کنید (نمونه در [`.env.example`](.env.example)):

| متغیر | توضیح |
| -------- | ----------- |
| `NEXT_PUBLIC_API_BASE_URL` | آدرس مطلق پایهٔ API پیام‌رسان، مانند `https://messenger.example.com`. اگر **تنظیم نشود**، اپ در «حالت گیت‌وی» اجرا می‌شود: تمام درخواست‌ها به مسیرهای نسبی `/api/v1/...` با پارامتر `XTransformPort` ارسال می‌شوند. |
| `NEXT_PUBLIC_MOCK_API_PORT` | پورت سرویس mock/گیت‌وی در حالت گیت‌وی (پیش‌فرض `3030`). |

آدرس WebSocket از همان پایه ساخته می‌شود (`/api/v1/ws`). تمام قراردادهای REST و WS توسط
`openapi.json` بکند تعریف شده‌اند — برای مستندات استقرار و API،
[مخزن بکند](https://github.com/Squsre/messenger-backend) را ببینید.

## 📁 ساختار پروژه

```text
src/
├── app/
│   ├── (auth)/            # ورود، ثبت‌نام، verify-email، reset-password
│   ├── (main)/            # پوستهٔ اپ + حالت خالی، c/[id]، g/[id]، تنظیمات، مخاطبین
│   ├── layout.tsx
│   └── globals.css
├── components/
│   ├── ui/                # اجزای پایه shadcn/ui
│   ├── layout/            # NavRail، BottomNav، ChatSidebar، InfoSidebar (پنل‌های پروفایل/گروه)
│   ├── chat/              # ChatViewport، MessageFeed، MessageItem، رسانه، ChatInput، EmojiPicker
│   ├── dialogs/           # چت جدید، گروه جدید، دعوت‌نامه‌ها، جستجوی گروه‌ها، CommandPalette
│   ├── settings/          # AboutPanel (بنر API / سلامت / readiness)
│   └── common/            # UserAvatar، StatusTick، EmptyState، providers
├── hooks/                 # use-media-url، use-mobile، use-typing، use-race-guard، use-unread-tab-title
├── lib/                   # api-client، validation (zod)، constants، media-url-cache، notifications، draft-store
├── stores/                # auth، conversation، message، socket، contacts، blocks، ui (+ reset registry)
├── types/                 # api.ts (از openapi.json)، ws.ts (قرارداد فریم)
└── proxy.ts               # گارد مسیر مبتنی بر کوکی (قرارداد middleware در Next 16)
public/
├── fonts/                 # وزیرمتن، Rajdhani، Cinzel Decorative (SIL OFL)
└── logo.svg، notification-icon-*.png، robots.txt
```

## 📐 نکات قرارداد با بکند

- طول عمر توکن دسترسی ۱۵ دقیقه است؛ هر پاسخ HTTP `401` یا بستن سوکت `4401/4403` نشست را پاک می‌کند و به `/login?expired=1` هدایت می‌کند.
- فریم احراز هویت WS به‌عنوان نخستین فریم در کمتر از ۵ ثانیه ارسال می‌شود؛ فریم‌های `ping` سرور فوراً پاسخ داده می‌شوند و کلاینت هر ۲۵ ثانیه ping می‌فرستد و اتصال‌های بیش از ۷۰ ثانیه ساکت را بازیافت می‌کند.
- فریم‌های «در حال تایپ» به یک عدد در هر ۱٫۵ ثانیه و فریم‌های خواندن به یک عدد در هر ثانیه محدود می‌شوند و هرگز `last_message_id` قدیمی‌تر تکرار نمی‌شود.
- رویدادهای `delivered`/`read` واترمارک هستند: هر پیامی با `id <= last_message_id` یک‌جا به‌روزرسانی می‌شود.
- سیاست رمز عبور: ۱۲ تا ۱۲۸ کاراکتر شامل حرف بزرگ، کوچک، رقم و کاراکتر خاص؛ سیاست نام کاربری: ۳ تا ۳۲ کاراکتر از حروف کوچک/رقم/زیرخط.
- پس از هر اتصال مجدد هر دو لیست بازخوانی می‌شوند تا شمارنده‌های نامحت‌شدهٔ سمت سرور همگام بمانند؛ همهٔ endpointهای مجموعه‌ای با `limit`/`offset` صفحه‌بندی می‌شوند و ادغام‌ها بر اساس id بدون تکرار انجام می‌شود.

## 📚 مستندات

- [`docs/UNIMPLEMENTED_ENDPOINTS.md`](docs/UNIMPLEMENTED_ENDPOINTS.md) — ممیزی کامل پوشش endpointها (صفحه‌بندی، ریس‌گارد جستجو، تاریخچهٔ دعوت‌نامه‌ها، سخت‌سازی آپلود)
- [`docs/AUDIT-v2.1.0-category-4-5.md`](docs/AUDIT-v2.1.0-category-4-5.md) و [`docs/AUDIT-v2.2.0-category-6.md`](docs/AUDIT-v2.2.0-category-6.md) — یادداشت‌های ممیزی نسخه‌ها

## 🤝 مشارکت

ایسیوها و پول‌ریکوئست‌ها خوش‌آمدند. لطفاً تغییرات را با قرارداد API بکند هماهنگ نگه دارید
و پیش از ارسال، `npm run lint` را اجرا کنید.

## 🔒 نکات امنیتی

- توکن‌های دسترسی هرگز در `localStorage` ذخیره نمی‌شوند؛ آن‌ها فقط در حافظه و یک کوکی `SameSite=Lax` (با `Secure` روی HTTPS) برای بازیابی نشست هنگام بوت نگه‌داری می‌شوند.
- تمام پیکربندی از طریق متغیرهای محیطی تأمین می‌شود — هیچ secretای در مخزن کامیت نشده است. برای مقادیر محلی، `.env.example` را به `.env.local` کپی کنید.

## 📄 مجوز

این پروژه تحت [مجوز MIT](LICENSE) منتشر شده است.
فونت‌های همراه پروژه (وزیرمتن، Rajdhani، Cinzel Decorative) تحت
[مجوز SIL Open Font](public/fonts/) عرضه شده‌اند.

---

<div align="center">

**فرانت‌اند پروژهٔ [Squsre/messenger-backend](https://github.com/Squsre/messenger-backend)** · ساخته‌شده با Next.js

</div>

</div>
