# nuclei-templates-bitrix

Nuclei templates for scanning websites built on CMS Bitrix.

## Layout

| Path | What |
|------|------|
| `templates/` | Regular checks (safe for bulk runs) |
| `enum/` | Login/admin enumeration (wordlist) — FP-guarded |
| `intrusive/` | **Active** probes that change state (upload/RCE). Not just “detect” — may write files. Run only with permission. |

## Usage

```bash
git clone https://github.com/krolchonok/bitrix-nuclei-templates.git
cd bitrix-nuclei-templates
```

Install nuclei: https://nuclei.projectdiscovery.io/docs/installation/

### Normal scan (no brute / no intrusive RCE)

```bash
nuclei -t templates/ -l TARGETS.txt
nuclei -t templates/ -u https://example.com
```

SSRF checks use Interactsh (`{{interactsh-url}}`) — keep OAST enabled (default).

### Account enumeration

```bash
nuclei -t enum/ -u https://example.com
```

- `Bitrix_Account_UIDH.yaml` — cookie `UIDH=deleted` (classic). First hits a random fake login; if that already returns `deleted`, wordlist is **skipped** (avoids “everyone exists” FP on modern Bitrix).
- `Bitrix_Admin_Captcha_Enum.yaml` — admin enum via CAPTCHA after failed logins (CVE-2020-28206), usually more reliable.

Wordlist: `enum/wordlists/unix_users.txt`.

### Intrusive (what it means)

`templates/` only **checks** (GET/read, OAST SSRF).  
`intrusive/` **exploits** — e.g. CVE-2022-27228 uploads a test file via the vote module. Use only on authorized targets.

```bash
nuclei -t intrusive/ -u https://example.com
```

## Notable templates

- `bitrix-detect` — tech detect
- `bitrix-updater-log` — `updater.log` / license key leak
- `bitrix-config-exposure` — `.settings.php` / `dbconn.php` (+ backups)
- `CVE-2020-13484` — URL preview SSRF (OAST)
- `bitrix-map-google-xss` — map settings XSS
- `intrusive/CVE-2022-27228` — vote module RCE

## License

MIT — see [LICENSE](LICENSE).
