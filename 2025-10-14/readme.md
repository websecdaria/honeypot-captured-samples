### wp-config.php.txt (modified)

**Brief:** An injection was found in `wp-config.php` that inserts a base64-decoded `<script src="//async.gsyndication.com/">` into the frontend. The injection deliberately excludes admin/API/AJAX paths (checks `is_admin` in local/sessionStorage), a common technique to hide malicious behavior from administrators.

**Indicators:** contains `base64_decode("PHNjcmlwdC...")`, external domains `async.gsyndication.com` (also observed: `yametric.com`, `mc.yandex.ru`).

**Likely purpose:** malvertising/loader — loads external JS which may perform redirects, deliver further payloads, or inject ads/malicious content.

### xml.php.txt (malicious backdoor)

**Brief:** PHP backdoor that requires a secret cookie/parameter (`f6975d6b0e6087dbea971c93cdce5dd2=da00c38a...`) to activate. When activated it collects host info (public IPs, hostname, users, cwd, /etc/passwd contents), appends SSH public keys to `authorized_keys` in user home dirs (persistence), attempts to fetch/execute code from `https://gsocket.io/y`, and exfiltrates collected data to `https://information.cloudsyndication.org/`.

**Indicators:** cookie/param name `f6975d6b0e6087dbea971c93cdce5dd2`, C2 `information.cloudsyndication.org`, fetch URL `gsocket.io/y`, hardcoded SSH public keys, use of `proc_open/exec/system/shell_exec`.

### wp-setting.php

**Brief:** WordPress backdoor that disables updates, injects code into plugins/themes, and sends admin data to `information.cloudsyndication.dev`.  
Executes PHP/shell commands via encoded cookies and can download or modify files remotely.  
Persistent and self-propagating.

**Indicators:**  
- Domain: `information.cloudsyndication.dev`  
- Functions: `base64_decode`, `wp_remote_request`, `add_action('admin_footer', ...)`  
- Markers: `_2869028782`, `_1314088273`, `_3243299888`

### wp-mailer.php

**Brief:** Simple mailer backdoor using PHP’s `mail()` and WordPress `wp_mail()` functions.  
Accepts base64-encoded parameters (`to`, `subject`, `message`) via cookies or request variables, enabling remote spam or phishing delivery through the infected site.

**Indicators:**  
- Parameters: `to`, `subject`, `message`  
- Functions: `mail()`, `wp_mail()`  
- Uses base64 decoding and hidden cookie keys (e.g. `1519e933e0f96b08752a95331d73ddba`)
