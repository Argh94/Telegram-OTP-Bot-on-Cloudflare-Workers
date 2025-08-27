# ☁️ Telegram OTP Bot on Cloudflare Workers

[![Cloudflare Workers](https://img.shields.io/badge/Cloudflare-Workers-orange?logo=cloudflare)](https://workers.cloudflare.com/)
[![Bash Script](https://img.shields.io/badge/Shell-Bash-blue?logo=gnubash)](https://www.gnu.org/software/bash/)
[![Telegram Bot](https://img.shields.io/badge/Telegram-Bot-2CA5E0?logo=telegram)](https://core.telegram.org/bots)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

این پروژه یک اسکریپت برای استقرار سریع و امن یک **ربات تلگرام** جهت دریافت کدهای OTP (یکبار مصرف) مبتنی بر `otpauth://` روی Cloudflare Workers است. این ربات پس از احراز هویت با رمز عبور، کدهای TOTP را با استفاده از Cloudflare KV به صورت موقت و امن ارائه می‌دهد.

**توجه:** این پروژه شامل داده‌های حساس (کلیدهای OTP شما) است. امنیت اکانت Cloudflare و Bot Token تلگرام را جدی بگیرید.

---

## ✨ ویژگی‌ها

- **رابط تلگرام:** دریافت OTPها فقط با چت تلگرام.
- **حفاظت با رمز عبور:** ربات فقط با رمز عبور قابل استفاده است (با محدودیت تعداد تلاش).
- **مدیریت نشست:** با Cloudflare KV و محدودیت زمانی برای هر کاربر.
- **خواندن اتوماتیک otpauth://:** وارد کردن گروهی URLهای TOTP از هر اپلیکیشن احراز هویت (مثل Google Authenticator).
- **تولید پویا OTP:** پشتیبانی از الگوریتم‌ها و پارامترهای مختلف TOTP (SHA1/SHA256/SHA512، تعداد رقم، بازه زمانی).
- **نصب و استقرار خودکار:** همه موارد از ساخت Worker تا تنظیمات، به صورت Bash Script.
- **بدون ذخیره رمزها در سورس:** رمز ربات و OTPها به صورت Secret ذخیره می‌شوند و در کد پروژه قرار نمی‌گیرند.

---

## 📦 پیش‌نیازها

- **حساب Cloudflare:** [ثبت‌نام](https://dash.cloudflare.com/sign-up)
- **Node.js و npm:** جهت نصب Wrangler
    ```bash
    node -v
    npm -v
    ```
- **نصب Wrangler CLI:**
    ```bash
    npm install -g wrangler
    ```
- **ورود به Wrangler:**
    ```bash
    wrangler login
    ```
- **توکن ربات تلگرام:** ساخت از [BotFather](https://t.me/botfather)
- **دریافت otpauth:// URLs:** خروجی گرفتن از اپ احراز هویت (مثلاً با [این ابزار](https://github.com/dim13/otpauth))

---

## ⚡ نصب و راه‌اندازی

1. **کلون یا دانلود پروژه:**
    ```bash
    git clone <REPO_URL>
    cd <REPO_DIR>
    ```
2. **اجرای اسکریپت نصب:**
    ```bash
    chmod +x install.sh
    ./install.sh
    ```
3. **پاسخ به سوالات اسکریپت:**
    - نام Worker (مثلاً: `my-otp-bot`)
    - نام KV Namespace (مثلاً: `OTP_BOT_SESSIONS`)
    - توکن ربات تلگرام
    - رمز عبور دسترسی کاربران
4. **وارد کردن otpauth://:**
    - اسکریپت فایل `auth_list.txt` می‌سازد.
    - تمام URLهای `otpauth://` را در آن (هر خط یک URL) قرار دهید.
    - پس از ذخیره، در ترمینال `go` را وارد و Enter بزنید.
5. **استقرار خودکار:**
    - خواندن URLها و تولید کد Worker
    - ساخت یا استخراج KV Namespace
    - ساخت `wrangler.toml`
    - ذخیره رمزها به صورت Secret
    - Deploy Worker
    - تنظیم اتوماتیک Webhook تلگرام به Worker جدید

---

---

## 🎮 استفاده

1. ربات را در تلگرام باز کنید و `/start` بفرستید.
2. رمز عبور را وارد کنید.
3. لیست سرویس‌های OTP شما نمایش داده می‌شود.
4. شماره مربوط به سرویس موردنظر را ارسال کنید.
5. کد OTP را دریافت می‌کنید. (برای هر بار دریافت باید `/start` بزنید)

---

## 🛡️ نکات امنیتی

- **رمز عبور قوی انتخاب کنید.**
- **توکن ربات تلگرام را به هیچ عنوان در جایی ذخیره نکنید یا به اشتراک نگذارید.**
- **Cloudflare خود را با رمز قوی و 2FA ایمن کنید.**
- **فایل `auth_list.txt` را پس از نصب حذف یا به جای امن منتقل کنید.**
- **آدرس Worker عمومی است؛ امنیت صرفاً به رمز عبور و API تلگرام وابسته است.**

---

## ⚙️ دستورات مهم

- **حذف Webhook تلگرام:**
  ```bash
  curl -X POST https://api.telegram.org/bot<YOUR_BOT_TOKEN>/deleteWebhook
  ```
- **بررسی Webhook فعلی:**
  ```bash
  curl https://api.telegram.org/bot<YOUR_BOT_TOKEN>/getWebhookInfo
  ```
- **بازنشر Worker پس از تغییر:**
  ```bash
  wrangler deploy
  ```

---

## 🚑 رفع اشکال و پشتیبانی

- **خطاهای Wrangler:** نسخه Wrangler را بروز کنید و مطمئن شوید وارد شده‌اید (`wrangler login`).
- **ربات پاسخ نمی‌دهد:** Webhook را بررسی کنید و لاگ Worker را با `wrangler tail` ببینید.
- **OTP اشتباه:** URLها را با دقت و بدون تغییر کپی کنید. ساعت سیستم را نیز دقیق تنظیم کنید.

---

## 💻 سازگاری سیستم عامل

این اسکریپت برای لینوکس و مک نوشته شده است. کاربران ویندوز می‌توانند از WSL (Windows Subsystem for Linux) استفاده کنند.

---

## 🙏 مشارکت

پیشنهاد، رفع باگ یا توسعه بیشتر؟ خوشحال می‌شویم Pull Request یا Issue ثبت کنید!

---

## 📝 لایسنس

[MIT](LICENSE)
