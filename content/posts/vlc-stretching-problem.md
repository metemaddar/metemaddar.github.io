---
date: '2025-09-20T16:31:46+03:30'
draft: false
title: 'کش اومدن صدای VLC و حل این مشکل'
cover:
    image: '/images/posts/mosafer-koochooloo-listening-to-vlc-nature.webp'
---
اگر شما هم گوش‌های بیش از حد حساسی داشته‌باشید، در هنگام شنیدن موسیقی با VLC این مشکل رو حس کردید که صدا گاهی اوقات کش میاد. شاید در حد چند میلی‌ثانیه باشه. این ممکنه برای شما آزاردهنده باشه.

من در ابتدا فکر می‌کردم که این مشکل منه و درست نمی‌شنوم. اما بعد متوجه شدم این در حقیقت یک فیچر در VLC هست که وقتی به درستی تنظیم نشه، برای برخی افراد می‌تونه به یک باگ تبدیل بشه.

این در واقع به ویژگی **Time Stretching** و **Resampling** در VLC مربوط می‌شه. VLC تلاش می‌کنه **sample rate** صدا رو با کارت صدا هماهنگ کنه.  
مثلا اگر sample rate موسیقی روی 44.1kHz باشه و کارت صدای شما روی 48kHz تنظیم شده‌باشه، VLC تلاش می‌کنه صدا رو با الگوریتم‌های Resampling با کارت صدا هماهنگ کنه.

برای رفع این مشکل دو راه داریم:  
- یا Resampling رو به کل **disable** کنیم.  
- یا از الگوریتم‌هایی با کیفیت بیشتر استفاده کنیم (مثل **SRC** یا **Speex Quality**). البته هرچه کیفیت بالاتر بره، پردازنده رو بیشتر درگیر می‌کنه.

---

## تنظیم دقیق در VLC
1. پنجره Preferences رو باز کن.  
   - از منو: `Tools → Preferences`  
   - یا کلید میانبر: `Ctrl + P`
1. پایین پنجره، حالت نمایش رو ببر روی **All** (به‌جای Simple).
2. از سمت چپ برو به:  

```
Audio → Output modules → Audio resampler
```


4. توی قسمت **Resampler module**، گزینه رو بذار روی:  
	- حالت `SRC (Secret Rabbit Code)` برای بالاترین کیفیت  
	- یا `Speex (quality)` برای تعادل بین کیفیت و مصرف CPU  

5. اگر می‌خوای stretching کامل خاموش بشه:  
	- برو به `Tools → Preferences → Audio`  
	- تیک `Enable Time-Stretching Audio` رو بردار.

---

## فهمیدن sample rate کارت صدا

روی لینوکس (با PulseAudio) می‌تونید بزنید:

```bash
pactl list sinks | grep -E 'Name|Sample Specification'
```
خروجی مثلاً اینطوری می‌شه:
```bash
Name: alsa_output.pci-0000_00_1f.3-platform-sof_sdw.HiFi__Speaker__sink
Sample Specification: s32le 2ch 48000Hz
```

اینجا کارت صدا روی 48000Hz کار می‌کنه.
## فهمیدن sample rate موسیقی در حال پخش

وقتی موسیقی رو در VLC پلی کردید، کلید Ctrl + J رو بزنید یا از منو بروید به:
```
Tools → Codec Information
```


در بخش Stream 0 می‌تونید ببینید آهنگ روی چه Sample Rate هست.
مثلاً:
```
Sample rate: 48000 Hz
Channels: Stereo
```

## نکته مهم
وقتی sample rate موسیقی و کارت صدا یکی باشه (مثلاً هر دو روی 48000Hz)، دیگه نیاز به Resampling نیست.
اینجاست که می‌تونید با خیال راحت:
- آپشن Time Stretching رو خاموش کنید
- و Resampling رو روی disable بذارید
- و مطمئن باشید صدا بدون تغییرهای ریز پخش می‌شه.

