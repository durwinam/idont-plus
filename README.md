
  <img src="Preview.png" alt="Preview.png" width="900">
</div>

<h1 align="center">Idont Sub template(PG)</h1>

<p align="center">
  نسخهٔ حرفه‌ای — قالب اشتراک با برند، زیرعنوان و لوگوی سفارشی
</p>

<p align="center">
  <a href="#نصب-خودکار">نصب خودکار</a> ·
  <a href="#نصب-دستی">نصب دستی</a> ·
  <a href="#سفارشی‌سازی-برند">سفارشی‌سازی برند</a> ·
  <a href="#تنظیمات-پنل">تنظیمات پنل</a> .
</p>

---

## ویژگی‌ها

- همهٔ امکانات نسخهٔ استاندارد
- نام برند، زیرعنوان و لوگوی سفارشی
- اعمال خودکار برندینگ با اسکریپت نصب
- رابط شیشه‌ای برای موبایل، تبلت و دسکتاپ
- یک فایل HTML — بدون Node.js و build

---

## نصب خودکار

روی سرور **Ubuntu** با Pasarguard نصب‌شده:

```bash
curl -fsSL https://raw.githubusercontent.com/durwinam/idont-plus/main/install.sh -o /tmp/idont-plus-install.sh && sudo bash /tmp/idont-plus-install.sh
```

یا:

```bash
wget -qO /tmp/idont-install.sh https://raw.githubusercontent.com/durwinam/idont-plus/main/install.sh && sudo bash /tmp/idont-plus-install.sh
```


در منو گزینه **1) idont-plus** را انتخاب کنید. سپس از شما پرسیده می‌شود:

- **نام برند** (FA و EN)
- **زیرعنوان / توضیح کوتاه** (FA و EN)
- **آدرس لوگو** (`https://` — قبل از نصب اعتبارسنجی می‌شود)

### اسکریپت چه کار می‌کند؟

1. منوی انتخاب نسخه (`Pro`)
2. برای Pro: دریافت اطلاعات برند و patch خودکار روی `index.html`
3. ذخیره در:

```text
/var/lib/pasarguard/templates/subscription/index.html
```

4. به‌روزرسانی `/opt/pasarguard/.env`:

```env
CUSTOM_TEMPLATES_DIRECTORY="/var/lib/pasarguard/templates/"
SUBSCRIPTION_PAGE_TEMPLATE="subscription/index.html"
```

5. اجرای `pasarguard restart`

**قرارداد قالب (این کلیدها را ثابت نگه دارید):**

```javascript
var DEFAULT_BRAND = {
  name: "...",
  subtitle: { fa: "...", en: "..." },
  logoUrl: "..."
};
```

شناسه‌های HTML اختیاری: `brand-title`، `brand-subtitle`، `brand-img`

> **پیش‌نیازها:** `wget`، `curl`، `python3`

---

## نصب دستی

### ۱. دانلود قالب

```bash
sudo mkdir -p /var/lib/pasarguard/templates/subscription/
sudo wget -N -O /var/lib/pasarguard/templates/subscription/index.html \
  https://raw.githubusercontent.com/durwinam/idont/main/index.html
```

### ۲. ویرایش برند (اختیاری)

فایل را باز کنید و بلوک `DEFAULT_BRAND` را ویرایش کنید:

```javascript
var DEFAULT_BRAND = {
  name: "نام برند شما",
  subtitle: {
    fa: "پنل اشتراک",
    en: "Subscription panel"
  },
  logoUrl: "https://example.com/logo.png"
};
```

### ۳. تنظیم Pasarguard

```bash
sudo nano /opt/pasarguard/.env
```

اضافه یا به‌روز کنید:

```env
CUSTOM_TEMPLATES_DIRECTORY="/var/lib/pasarguard/templates/"
SUBSCRIPTION_PAGE_TEMPLATE="subscription/index.html"
```

### ۴. راه‌اندازی مجدد

```bash
sudo pasarguard restart
```

> برای اعمال خودکار برندینگ، از **اسکریپت نصب** استفاده کنید.

---

## سفارشی‌سازی برند

### در کد (`index.html`)

```javascript
var DEFAULT_BRAND = {
  name: "durwinam Template",
  subtitle: {
    fa: "پنل اشتراک",
    en: "Subscription panel"
  },
  logoUrl: ""
};
```

### Inject قبل از لود صفحه

```html
<script>
  window.durwinam_BRAND = {
    name: "نام برند شما",
    subtitle: { fa: "زیرعنوان", en: "Your tagline" },
    logoUrl: "https://example.com/logo.png"
  };
</script>
```

---

## تنظیمات پنل

1. پنل Pasarguard → **Settings → Subscription**
2. ویرایش **announcement** و **announcement link**
3. افزودن/ویرایش اپ‌ها در بخش apps

---
