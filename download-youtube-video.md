---

```markdown
# راهنمای دانلود ویدیو از YouTube به Google Drive با استفاده از Colab

> ⚠️ **پیش‌نیاز بسیار مهم برای دسترسی به گوگل درایو:**
> برای جلوگیری از خطاهای اتصال به گوگل درایو در محیط کولب و دور زدن محدودیت‌های شبکه‌ای، اجرای مراحل لینک زیر پیش از شروع کار الزامی است:
> 🔗 [راهنمای MITM-DomainFronting برای دسترسی به سرویس‌ها](https://github.com/patterniha/MITM-DomainFronting)

با استفاده از این روش می‌توانید با سرعت سرورهای گوگل، ویدیوهای یوتیوب را دانلود کرده و مستقیم در درایو خود ذخیره کنید. ما برای این کار از کتابخانه قدرتمند `yt-dlp` استفاده می‌کنیم.

## ۱. آماده‌سازی
1. وارد سایت [Google Colab](https://colab.research.google.com/) شوید و یک نوتبوک جدید بسازید.
2. لینک ویدیوی یوتیوب مورد نظر خود را کپی کنید.

## ۲. کد اجرایی دانلود
کد زیر را در یک سلول کولب کپی کنید. تنظیمات مورد نظر را طبق راهنمای انتهای فایل اعمال کرده و اجرا کنید:

```python
# الف) اتصال به گوگل درایو
from google.colab import drive
drive.mount('/content/drive')

# ب) نصب کتابخانه yt-dlp
!pip install -U yt-dlp

import os

# ----------------- تنظیمات پارامترها -----------------
# ۱. لینک ویدیوی یوتیوب را اینجا قرار دهید
VIDEO_URL = "[https://www.youtube.com/watch?v=xxxxxxx](https://www.youtube.com/watch?v=xxxxxxx)"

# ۲. مسیر ذخیره فایل در گوگل درایو
DOWNLOAD_PATH = "/content/drive/MyDrive/YouTube_Downloads"

# ۳. تنظیم کیفیت و فرمت (به راهنمای پایین فایل مراجعه کنید)
# گزینه پیش‌فرض: بهترین کیفیت ویدیو mp4 + بهترین صدا
FORMAT_SETTING = "bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best"

# ۴. آیا فقط صدا (MP3) استخراج شود؟ (True یا False)
ONLY_AUDIO = False
# -----------------------------------------------------

os.makedirs(DOWNLOAD_PATH, exist_ok=True)

print("🚀 در حال دریافت اطلاعات و شروع دانلود...")

if ONLY_AUDIO:
    # دستور دانلود فقط صدا با فرمت mp3
    !yt-dlp -o "{DOWNLOAD_PATH}/%(title)s.%(ext)s" -x --audio-format mp3 {VIDEO_URL}
else:
    # دستور دانلود ویدیو با کیفیت انتخاب شده
    !yt-dlp -o "{DOWNLOAD_PATH}/%(title)s.%(ext)s" -f "{FORMAT_SETTING}" {VIDEO_URL}

print("✅ عملیات با موفقیت انجام شد و فایل در گوگل درایو ذخیره گشت!")
⚙️ راهنمای تغییر پارامترها و تنظیمات حرفه‌ای
۱. تغییر کیفیت ویدیو (FORMAT_SETTING)
شما می‌توانید با تغییر این مقدار، کیفیت دانلود را کنترل کنید:

بهترین کیفیت ممکن (بدون محدودیت حجم):

Python
FORMAT_SETTING = "bestvideo+bestaudio/best"
محدود کردن به کیفیت 1080p (برای جلوگیری از حجم زیاد ویدیوهای 4K):

Python
FORMAT_SETTING = "bestvideo[height<=1080][ext=mp4]+bestaudio[ext=m4a]/best[height<=1080][ext=mp4]/best"
کیفیت معمولی 720p (مناسب برای اشتراک‌گذاری سریع):

Python
FORMAT_SETTING = "bestvideo[height<=720][ext=mp4]+bestaudio[ext=m4a]/best[height<=720][ext=mp4]/best"
۲. دانلود فقط صدا (ONLY_AUDIO)
اگر می‌خواهید از یوتیوب به عنوان منبع پادکست یا موزیک استفاده کنید:

مقدار ONLY_AUDIO = True را قرار دهید.

در این حالت، اسکریپت به طور خودکار ویدیو را دانلود کرده و صدا را با فرمت MP3 استخراج می‌کند و فایل اصلی ویدیو را حذف می‌کند.

۳. تغییر مسیر و نام فایل (DOWNLOAD_PATH)
متغیر DOWNLOAD_PATH محل ذخیره را تعیین می‌کند.

عبارت %(title)s در کد باعث می‌شود نام فایل دقیقاً همان عنوان ویدیو در یوتیوب باشد. اگر می‌خواهید نام خاصی بگذارید، می‌توانید در بخش کد %(title)s را با نام دلخواه جایگزین کنید.

۴. دانلود لیست پخش (Playlist)
اگر به جای لینک یک ویدیو، لینک یک Playlist را در VIDEO_URL قرار دهید، این اسکریپت تمام ویدیوهای آن لیست را یکی پس از دیگری در پوشه درایو شما دانلود خواهد کرد.
