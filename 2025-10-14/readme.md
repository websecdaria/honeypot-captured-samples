### wp-config.php.txt (modified)

**Brief:** An injection was found in `wp-config.php` that inserts a base64-decoded `<script src="//async.gsyndication.com/">` into the frontend. The injection deliberately excludes admin/API/AJAX paths (checks `is_admin` in local/sessionStorage), a common technique to hide malicious behavior from administrators.

**Indicators:** contains `base64_decode("PHNjcmlwdC...")`, external domains `async.gsyndication.com` (also observed: `yametric.com`, `mc.yandex.ru`).

**Likely purpose:** malvertising/loader — loads external JS which may perform redirects, deliver further payloads, or inject ads/malicious content.
