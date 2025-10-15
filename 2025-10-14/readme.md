### wp-config.php.txt (modified)

**Brief:** An injection was found in `wp-config.php` that inserts a base64-decoded `<script src="//async.gsyndication.com/">` into the frontend. The injection deliberately excludes admin/API/AJAX paths (checks `is_admin` in local/sessionStorage), a common technique to hide malicious behavior from administrators.

**Indicators:** contains `base64_decode("PHNjcmlwdC...")`, external domains `async.gsyndication.com` (also observed: `yametric.com`, `mc.yandex.ru`).

**Likely purpose:** malvertising/loader — loads external JS which may perform redirects, deliver further payloads, or inject ads/malicious content.

### xml.php.txt (malicious backdoor)

**Brief:** PHP backdoor that requires a secret cookie/parameter (`f6975d6b0e6087dbea971c93cdce5dd2=da00c38a...`) to activate. When activated it collects host info (public IPs, hostname, users, cwd, /etc/passwd contents), appends SSH public keys to `authorized_keys` in user home dirs (persistence), attempts to fetch/execute code from `https://gsocket.io/y`, and exfiltrates collected data to `https://information.cloudsyndication.org/`.

**Indicators:** cookie/param name `f6975d6b0e6087dbea971c93cdce5dd2`, C2 `information.cloudsyndication.org`, fetch URL `gsocket.io/y`, hardcoded SSH public keys, use of `proc_open/exec/system/shell_exec`.
