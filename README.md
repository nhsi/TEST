# راه‌اندازی نسخهٔ تمرینی مه یاس

`preview/index.html` را می‌توانید مستقیماً در مرورگر باز کنید. مسیرهای عمومی نمونه، فرم اعتبارسنجی محلی و نمای حساب در حالت غیرفعال همراه پیش‌نمایش هستند. تصاویر در `evidence/` و گزارش در `report.html` قرار دارند.

## وردپرس با پایگاه دادهٔ جدید

پوشهٔ `wordpress/site` ریشهٔ وب است. فایل `wordpress/content.sql.gz` را بیرون از ریشهٔ وب نگه دارید. برای اجرای تمرینی به PHP با mysqli، یک پایگاه دادهٔ خالی MySQL یا MariaDB با پشتیبانی از utf8mb4_unicode_520_ci، و WP-CLI نیاز است. نسخهٔ PHP 8.3 برای آزمون در نظر گرفته شد. بسته بر اساس فایل‌های قابل استخراج از آرشیو کاربر ساخته شده است.

1. فایل SQL را از GZIP خارج کنید. از داخل `wordpress/site` پیکربندی را ایجاد کنید. نام نمونهٔ دیتابیس `mahyas_local` و نام نمونهٔ کاربر `mahyas_local` است. مقادیر را با محیط خود تنظیم کنید. رمز از طریق prompt وارد می‌شود.

   ```sh
   wp config create --dbname=mahyas_local --dbuser=mahyas_local --dbhost=127.0.0.1 --dbprefix=wp_ --prompt=dbpass
   wp config set WP_ENVIRONMENT_TYPE local
   wp config set WP_HTTP_BLOCK_EXTERNAL true --raw
   wp config set DISABLE_WP_CRON true --raw
   wp db import ../content.sql
   ```

2. آدرس اصلی را در داده‌های سریال‌شده هم جایگزین کنید. حساب مدیر تازه بسازید. نام و رمز محیط اصلی در dump تحویلی وجود ندارد. برای رمز مدیر تازه از prompt استفاده کنید.

   ```sh
   wp search-replace 'https://myg.co.ir' 'http://127.0.0.1:8080' --all-tables-with-prefix --skip-columns=guid --precise
   wp user create challenge_admin preview@example.invalid --role=administrator --prompt=user_pass
   wp plugin activate elementor elementor-pro nextend-smart-slider3-pro font-farsi mahyas-refresh
   wp rewrite flush
   wp server --host=127.0.0.1 --port=8080
   ```

3. به `http://127.0.0.1:8080/` بروید. پنل مدیر در `/wp-admin/` است. مسیرهای بررسی `/?page_id=660` برای محصولات، `/?page_id=10048` برای مشاوره، `/?page_id=9` برای حساب و `/?page_id=20` برای تماس‌اند.
4. یک درخواست تمرینی با ایمیل رزروشدهٔ `preview@example.invalid` ثبت کنید. در `Elementor → Submissions` ثبت را بررسی کنید. افزونهٔ MU در محیط `local` تحویل ایمیل را شبیه‌سازی می‌کند تا پیام خارجی ارسال نشود. آزمون ارسال واقعی ایمیل به محیط ایمیل تست نیاز دارد.
5. WooCommerce مانند تنظیم اولیهٔ آرشیو غیرفعال می‌ماند. صفحات حساب، سبد و پرداخت در این حالت پیام پیگیری دارند. اگر در کپی جداگانه WooCommerce فعال شود، شورت‌کدهای خود افزونه اولویت دارند. سفارش، پرداخت و کاربر اصلی از پشتیبان وارد نشده‌اند.

برای بازگشت به قالب و رفتار قبلی، `wp plugin deactivate mahyas-refresh` را اجرا کنید. داده‌های اصلی Elementor با فعال شدن افزونه بازنویسی نمی‌شوند. نسخهٔ وردپرس، اتصال دیتابیس و ورود مدیر در محیط این تحویل اجرا و تأیید نشده‌اند. تست‌های انجام‌شده فقط مربوط به پیش‌نمایش HTML و بررسی محتوای بسته‌اند.
