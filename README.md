# nuclei-templates-bitrix

Nuclei templates for scanning websites built on CMS Bitrix.

## Layout

| Path | What |
|------|------|
| `templates/` | Regular checks (safe for bulk runs) |
| `enum/` | UIDH login bruteforce — **not** included in normal scans |

## Usage

```bash
git clone https://github.com/jhonnybonny/bitrix-nuclei-templates.git
cd bitrix-nuclei-templates
```

Install nuclei: https://nuclei.projectdiscovery.io/docs/installation/

### Normal scan (no login brute)

```bash
nuclei -t templates/ -l TARGETS.txt
nuclei -t templates/ -u https://example.com
```

### Account enumeration (UIDH + Unix wordlist)

```bash
nuclei -t enum/ -u https://example.com
```

Wordlist: `enum/wordlists/unix_users.txt` (~300 Unix/service/Bitrix logins).
Valid login → response sets `BITRIX_SM_UIDH=deleted`.

## Customization

See https://nuclei.projectdiscovery.io/docs/writing-templates/

## License

MIT — see [LICENSE](LICENSE).
