# راهنمای فارسی Grill Me و Grill With Docs برای Codex و Claude Code

> استخراج دانش پنهان تیم، شفاف‌سازی تصمیم‌های فنی و مستندسازی بخشی از دانش پروژه پیش از سپردن توسعه به ایجنت‌های هوش مصنوعی.

این راهنما برای کسانی است که زیر پست اینستاگرام کلمه **«حافظه»** را کامنت کرده‌اند و می‌خواهند استفاده از دو Skill متن‌باز **Grill Me** و **Grill With Docs** را روی یک پروژه موجود شروع کنند.

**منبع اصلی:** [مخزن Matt Pocock](https://github.com/mattpocock/skills) — این ریپوی آموزشی، ترجمه و راهنمای مستقل است؛ سازنده اسکیل‌ها نیست.

## چرا به این دو Skill نیاز داریم؟

فرض کنید چند سال روی یک نرم‌افزار کار کرده‌اید. بعضی تصمیم‌ها در سورس‌کد یا مستندات دیده نمی‌شوند: مثلاً یک API قدیمی هنوز توسط مشتریان بیرونی استفاده می‌شود، با اینکه در کد فعلی هیچ مصرف‌کننده داخلی ندارد. ممکن است یک ایجنت بدون اطلاع از این وابستگی، حذف آن API را پیشنهاد دهد.

به این بخش از دانش تجربی و مستندنشده **دانش ضمنی (Tacit Knowledge)** می‌گوییم. هدف این دو اسکیل کمک به کشف ابهام‌ها و استخراج بخشی از این دانش از طریق گفت‌وگوست؛ نه ساخت خودکار یک پایگاه داده یا یک Second Brain کامل.

| Skill | کاربرد | خروجی پیش‌فرض |
|---|---|---|
| `grill-me` | مصاحبه برای روشن کردن نیازها، فرضیات، تصمیم‌ها و محدودیت‌ها؛ حتی بدون ریپوی کد | گفت‌وگو؛ به‌تنهایی فایل دائمی تولید نمی‌کند |
| `grill-with-docs` | همان مصاحبه در زمینه یک ریپو، همراه با ثبت زبان مشترک و تصمیم‌های معماری واجد شرایط | `GLOSSARY.md` و، در صورت لزوم، ADR در `docs/adr/` |

> **توجه به نسخه جدید:** موتور اصلی مصاحبه Skill به نام `grilling` است. در نسخه فعلی ممکن است چند سؤال مستقل در هر دور پرسیده شود، نه الزاماً یک سؤال در هر پیام. سؤال‌های وابسته به پاسخ قبلی به دور بعد منتقل می‌شوند.

## ۱. پیش‌نیازها

- Codex CLI یا Claude Code که قبلاً نصب و راه‌اندازی شده باشد.
- Node.js و `npx` فقط برای روش نصب `skills` CLI.
- برای `grill-with-docs`: یک پوشه پروژه یا Git repository که مجاز باشید در آن مستندات ایجاد کنید.
- قبل از اجرا روی پروژه اصلی، یک Branch جدا بسازید و خروجی‌ها را بازبینی کنید.

```bash
cd /path/to/your-project
git switch -c docs/knowledge-discovery
git status --short
```

اگر ریپو تغییرات ذخیره‌نشده دارد، قبل از اجرای Skill آن‌ها را جداگانه مدیریت کنید.

## ۲. نصب Skillها روی Codex

### روش الف: نصب با Skills CLI (قابل استفاده برای نصب انتخابی)

در پوشه پروژه دستور زیر را اجرا کنید:

```bash
npx skills@latest add mattpocock/skills -a codex
```

در مرحله انتخاب Skillها **هر چهار مورد** زیر را انتخاب کنید:

```text
grill-me
grill-with-docs
grilling
domain-modeling
```

**چرا چهار مورد؟** `grill-me` به `grilling` وابسته است و `grill-with-docs` به `grilling` و `domain-modeling` نیاز دارد. فقط نصب دو نام اول ممکن است منجر به اجرا نشدن رفتار اصلی شود.

می‌توانید ابتدا انتخاب‌های موجود را ببینید:

```bash
npx skills@latest list
```

اگر نصب به‌صورت سراسری یا محلی انتخاب می‌کنید، دقت کنید مسیر نصب با محیط Codex فعلی سازگار باشد. سپس Codex را مجدداً راه‌اندازی کنید.

### روش ب: پلاگین رسمی مجموعه (جایگزین روش الف)

اگر نسخه Codex شما از دستورهای Plugin پشتیبانی می‌کند:

```bash
codex plugin marketplace add mattpocock/skills
codex plugin add mattpocock-skills@mattpocock
```

**یکی از دو روش را انتخاب کنید** تا اسکیل‌ها تکراری نصب نشوند. روش Plugin کل مجموعه را نصب می‌کند؛ روش CLI برای انتخاب فایل‌های اسکیل و به‌روزرسانی دستی مناسب است.

## ۳. نصب روی Claude Code

### روش الف: پلاگین رسمی

```bash
claude plugin install mattpocock-skills@claude-plugins-official
```

این دستور در مستندات سازنده معرفی شده است. اگر محیط شما این Marketplace را پیدا نکرد، از روش `npx` زیر استفاده کنید.

### روش ب: نصب انتخابی با Skills CLI

```bash
npx skills@latest add mattpocock/skills -a claude-code
```

در انتخاب‌ها همان چهار Skill را فعال کنید: `grill-me`، `grill-with-docs`، `grilling` و `domain-modeling`. سپس Claude Code را مجدداً راه‌اندازی کنید. هر دو روش را هم‌زمان استفاده نکنید.

## ۴. تنظیم و بررسی فعال شدن Skillها

### در Codex

داخل پروژه اجرا کنید:

```bash
codex
```

در پیام Codex از نام واضح Skill استفاده کنید، برای مثال:

```text
$grill-me

Help me clarify the undocumented decisions behind our existing project.
Do not implement changes.
```

در برخی رابط‌ها می‌توانید Skill را با تایپ `$` از فهرست انتخاب کنید. اگر رابط نصب‌شده شما دستور اسلش مخصوص Skill را ارائه می‌کند، از همان ورودی استفاده کنید.

### در Claude Code

داخل پروژه اجرا کنید:

```bash
claude
```

سپس یکی از دستورهای زیر را وارد کنید:

```text
/grill-me
```

یا:

```text
/grill-with-docs
```

> **عیب‌یابی مهم در Codex:** طبق گزارش ثبت‌شده در مخزن اصلی، برخی نسخه‌ها از دستور داخلی `Skill tool` که در متن این Skillها آمده پشتیبانی نمی‌کنند. اگر فقط عنوان Skill فعال شد ولی مصاحبه واقعی شکل نگرفت، صریحاً از ایجنت بخواهید Skillهای وابسته را از فایل `SKILL.md` بارگذاری کند. مسیر رایج در نصب سراسری `~/.agents/skills/` است، اما مسیر واقعی را مطابق نتیجه نصب خودتان بررسی کنید.

پرامپت عیب‌یابی:

```text
Before continuing, load and follow the installed grilling skill.
For grill-with-docs, also load and follow domain-modeling.
If your runtime has no Skill tool, locate and read their SKILL.md files directly.
Tell me which files you loaded. Do not pretend a skill was loaded if it was not.
```

## ۵. سناریوی عملی: دانش پنهان یک پروژه قدیمی

**مسئله:** در پروژه ما یک API قدیمی وجود دارد که هیچ بخش داخلی فعلی از آن استفاده نمی‌کند. ولی اعضای قدیمی تیم می‌دانند چند مشتری بیرونی هنوز به آن وابسته‌اند. این موضوع در کد و مستندات ثبت نشده است.

### مرحله اول — مصاحبه با Grill Me

برای شروع بدون تغییر فایل‌ها، در Codex یا Claude Code `grill-me` را فراخوانی کنید و این پرامپت را بدهید:

```text
We have maintained an existing software product for several years.
Now we want to use AI coding agents to make future changes safely.

Run the grill-me / grilling interview to uncover:
- undocumented business rules
- legacy customer dependencies
- reasons behind historical technical decisions
- constraints that cannot be learned from source code alone
- unresolved questions and risky assumptions

Start with our apparently unused legacy API.
Ask in focused rounds, explain your recommended answer for each decision,
and wait for my responses before moving to dependent decisions.
Do not write code or change files.
```

در مصاحبه واقعیت‌های پروژه را توضیح دهید؛ اگر تعداد مشتریان یا مسیر استفاده را نمی‌دانید، از ایجنت بخواهید آن را **موضوع باز برای بررسی** ثبت کند، نه یک واقعیت قطعی.

**خروجی مورد انتظار:** شفاف‌تر شدن دامنه مسئله و تصمیم‌ها در گفت‌وگو. `grill-me` به‌تنهایی حافظه دائمی نمی‌سازد.

### مرحله دوم — مستندسازی با Grill With Docs

در یک ریپوی واقعی، `grill-with-docs` را فعال کرده و این پرامپت را ارسال کنید:

```text
We are preparing this existing repository for AI-assisted development.

Use grill-with-docs, and make sure grilling and domain-modeling are loaded.
Inspect the codebase and current documentation before asking questions.

Interview us to uncover undocumented business rules, historical decisions,
legacy API consumers, operational constraints, and domain terminology.
Distinguish confirmed facts, assumptions, and open questions.
Do not invent customer dependencies or undocumented history.

Maintain the project's agreed terminology in GLOSSARY.md.
Create an ADR under docs/adr/ only for decisions that qualify under
this skill's criteria. Explain why each ADR does or does not qualify.

Do not change application source code, configurations, or infrastructure.
Pause for our answers and confirmation before finalizing conclusions.
```

**نکته:** `GLOSSARY.md` دفتر ثبت همه تصمیم‌ها نیست. فقط اصطلاحات تثبیت‌شده وارد آن می‌شوند. تصمیم‌های مهمی که برگشت‌پذیری دشوار، غافلگیرکننده‌بودن بدون Context و بده‌بستان واقعی داشته باشند ممکن است ADR شوند. امکان دارد هیچ ADRای ایجاد نشود؛ این الزاماً به معنی خرابی Skill نیست.

### مرحله سوم — کنترل فایل‌ها

```bash
git status --short
git diff -- GLOSSARY.md docs/adr/
```

بررسی کنید که صرفاً مستندات موردنظر تغییر کرده‌اند و هر ادعای جدید با صحبت اعضای تیم یا منبع معتبر پشتیبانی می‌شود.

### مرحله چهارم — یک Session جدید باز کنید و حافظه را آزمایش کنید

```text
Before proposing changes to the legacy API, inspect GLOSSARY.md,
relevant ADRs, and the existing API implementation.

Explain what is known about external consumers, what remains uncertain,
and which checks must pass before removal or modification.
Cite repository files. Do not change code.
```

اگر ایجنت به مستندات مراجعه نکرد، مسیر مستندات را در `AGENTS.md` برای Codex یا `CLAUDE.md` برای Claude Code معرفی کنید؛ معرفی مسیرها را کوتاه نگه دارید، نه اینکه همه دانش را در آن فایل‌ها کپی کنید.

## ۶. نمونه تنظیمات سبک برای هر پروژه

این دو فایل را **فقط در صورت نیاز** داخل ریپوی خود ایجاد یا تکمیل کنید. لازم نیست محتوای هر دو کاملاً یکسان باشد؛ نمونه‌ها فقط مسیر دانش را معرفی می‌کنند.

**نمونه `AGENTS.md` برای Codex:**

```md
# Project Knowledge

Before proposing risky changes, read relevant docs in:
- GLOSSARY.md
- docs/adr/

Treat undocumented assumptions as unverified. Ask before breaking public APIs.
Do not modify implementation while conducting knowledge-discovery interviews.
```

**نمونه `CLAUDE.md` برای Claude Code:**

```md
# Project Context

Use GLOSSARY.md for agreed domain terms and docs/adr/ for qualifying
architectural decisions. Verify old decisions against current code.
Ask for evidence when an external dependency is not documented.
Do not make code changes during a grill-with-docs knowledge interview.
```

این تنظیمات خودشان Skill را نصب یا اجرا نمی‌کنند؛ تنها به ایجنت یاد می‌دهند هنگام کار از چه مستنداتی استفاده کند.

## ۷. خطاهای رایج

| مشکل | علت احتمالی | راه‌حل |
|---|---|---|
| Skill اجرا نمی‌شود | وابستگی‌های `grilling` یا `domain-modeling` نصب نشده‌اند | چهار Skill را نصب و Agent را مجدداً اجرا کنید |
| Codex فقط حرف می‌زند، رفتار Skill را ندارد | زیر-Skillها بارگذاری نشده‌اند یا ابزار داخلی Skill وجود ندارد | از آن بخواهید فایل‌های `SKILL.md` وابستگی‌ها را مستقیم بخواند |
| به‌جای مصاحبه بلافاصله کد می‌نویسد | اجرای workflow روشن نشده یا خروجی مطلوب نامشخص است | بگویید تا تأیید شما هیچ فایل اجرایی تغییر نکند |
| هیچ ADR ساخته نشده | تصمیمی واجد شرایط سخت‌گیرانه ADR نبوده | معیارهای ADR را بررسی کنید؛ ساخت فایل را تحمیل نکنید |
| فقط بخشی از گفتگو مستند شده | طراحی Skill مستندسازی کامل مصاحبه نیست | برای بقیه تصمیم‌ها یک مستند جدا، پس از تأیید انسان، تهیه کنید |
| اسکیل پس از نصب دیده نمی‌شود | نصب در scope متفاوت بوده یا Session قبلی باز است | محل نصب را بررسی و Codex/Claude Code را مجدداً اجرا کنید |

## ۸. آیا این کار به‌تنهایی Second Brain می‌سازد؟

خیر. این دو Skill **ورودی ارزشمندی برای مغز دوم** فراهم می‌کنند. برای داشتن حافظه سازمانی ماندگار، باید درباره طبقه‌بندی منابع، تاریخ اعتبار اطلاعات، مالکیت مستندات، دسترسی کاربران و روش بازیابی دانش هم تصمیم بگیرید. ممکن است Markdown و Wiki کافی باشند؛ برای نیازهای خاص می‌توانید بعداً Semantic Search، RAG یا Knowledge Graph اضافه کنید.

**قاعده کاربردی:** اول دانش را معتبر و قابل مراجعه ثبت کنید؛ بعد سراغ زیرساخت پیچیده بازیابی بروید.

## ۹. منابع اصلی و به‌روزرسانی

- [مخزن رسمی اسکیل‌ها](https://github.com/mattpocock/skills)
- [راهنمای Grill Me](https://github.com/mattpocock/skills/blob/main/docs/productivity/grill-me.md)
- [راهنمای Grilling](https://github.com/mattpocock/skills/blob/main/docs/productivity/grilling.md)
- [راهنمای Grill With Docs](https://github.com/mattpocock/skills/blob/main/docs/engineering/grill-with-docs.md)
- [گزارش مشکل بارگذاری Skillهای وابسته در Codex](https://github.com/mattpocock/skills/issues/1123)

این راهنما بر اساس وضعیت مستندات در **۱۲ اکتبر ۲۰۲۶** تنظیم شده است. نام دستورها و جزئیات Skillها ممکن است در نسخه‌های آینده تغییر کنند؛ هنگام نصب، README و docs مخزن سازنده را نیز بررسی کنید.

---

**پیام پیشنهادی ارسال در دایرکت:** «این راهنمای فارسی نصب و استفاده از گریل می و گریل ویت داکس روی کدکس و کلاد کده. داخلش لینک اصلی اسکیل‌ها، نصب، کانفیگ پروژه، پرامپت آماده و سناریوی استخراج دانش پنهان تیم قرار داره.»
