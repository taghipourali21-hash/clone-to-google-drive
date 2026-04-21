# راهنمای جامع دانلود مدل‌های Hugging Face در Google Drive با استفاده از Colab

> ⚠️ **پیش‌نیاز بسیار مهم برای دسترسی به گوگل درایو:**
> برای اطمینان از دسترسی بدون مشکل به سرویس‌های گوگل و دور زدن محدودیت‌های احتمالی شبکه که مانع اتصال (Mount) شدن درایو می‌شوند، حتماً پیش از هر کاری راهنمای زیر را مطالعه و روی شبکه خود اعمال کنید:
> 🔗 [راهنمای MITM-DomainFronting برای دسترسی به سرویس‌ها](https://github.com/patterniha/MITM-DomainFronting)

این راهنما به شما نشان می‌دهد که چگونه هر مدلی را از سایت Hugging Face بدون مصرف اینترنت و حجم سیستم شخصی، مستقیماً به فضای گوگل درایو خود منتقل کنید.

## قدم اول: آماده‌سازی Google Colab
1. وارد سایت [Google Colab](https://colab.research.google.com/) شوید.
2. یک نوتبوک جدید (New Notebook) ایجاد کنید.

## قدم دوم: پیدا کردن شناسه مدل (Repository ID)
1. به سایت Hugging Face بروید و مدل مورد نظر خود را پیدا کنید.
2. شناسه مدل که در بالای صفحه نوشته شده است را کپی کنید. 
   *(مثال: `stabilityai/stable-diffusion-3.5-medium` یا `meta-llama/Llama-2-7b`)*

## قدم سوم: دریافت توکن (برای مدل‌های محافظت‌شده)
اگر مدلی که می‌خواهید دانلود کنید نیازمند تایید قوانین (Gated) است:
1. در صفحه مدل روی **Agree and access repository** کلیک کنید.
2. از بخش پروفایل خود به `Settings > Access Tokens` بروید و یک توکن کپی کنید.

## قدم چهارم: اجرای کد در کولب
کد زیر را در یک سلول (Cell) از کولب کپی کرده، متغیرهای آن را با اطلاعات خود جایگزین کنید و دکمه Play را بزنید:

```python
# ۱. اتصال به گوگل درایو
from google.colab import drive
drive.mount('/content/drive')

# ۲. نصب ابزار رسمی Hugging Face
!pip install -U "huggingface_hub[cli]"

import os

# ----------------- تنظیمات -----------------
# شناسه مدل را اینجا قرار دهید
REPO_ID = "نام_کاربری/نام_مدل" 

# مسیر ذخیره در گوگل درایو (می‌توانید تغییر دهید)
DOWNLOAD_PATH = "/content/drive/MyDrive/HuggingFace_Models"

# توکن خود را اینجا بگذارید (اگر مدل عمومی است، خالی بگذارید)
HF_TOKEN = "توکن_شما_در_صورت_نیاز"
# ---------------------------------------------

os.makedirs(DOWNLOAD_PATH, exist_ok=True)

print(f"🚀 در حال دانلود مدل {REPO_ID} ...")

# ۳. دستور دانلود کل مخزن
if HF_TOKEN:
    !huggingface-cli download {REPO_ID} --local-dir {DOWNLOAD_PATH}/{REPO_ID.replace('/', '_')} --token {HF_TOKEN}
else:
    !huggingface-cli download {REPO_ID} --local-dir {DOWNLOAD_PATH}/{REPO_ID.replace('/', '_')}

print("✅ دانلود با موفقیت انجام شد و در گوگل درایو شما قرار گرفت!")
