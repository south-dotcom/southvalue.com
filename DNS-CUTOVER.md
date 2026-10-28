# Finish the South Value domain migration in OVH

The cleaned site is deployed in [south-dotcom/southvalue.com](https://github.com/south-dotcom/southvalue.com). GitHub Pages serves `main` → `/docs`. The first deployment succeeded, and `www.southvalue.com` is saved as the custom domain.

**Migration completed on 28 October 2026 at approximately 18:14 Berlin time.** OVH DNS is verified, GitHub DNS validation passed, the certificate is active for both names, and Enforce HTTPS is enabled. The apex redirects to `https://www.southvalue.com/`. Fresh HTTP page and asset requests redirect to HTTPS; old cached HTTP homepage responses may take a few minutes to expire. The instructions below are retained as a migration record.

The following records describe the **previous host, before migration**, for rollback reference:

- Nameservers: `dns109.ovh.net` and `ns109.ovh.net` (keep these).
- `southvalue.com` A: `51.15.191.141`.
- `www.southvalue.com` CNAME: `southvalue.com`.
- No apex AAAA record was returned.

## Enter the new records

In OVHcloud Manager, open **Web Cloud → Domain names → southvalue.com → DNS zone**. Use the row menu **⋮ → Edit the entry** to change existing records and **Add an entry** for the additional A records. Keep TTL at its default unless you have a specific preference.

1. Edit the existing `www` CNAME from `southvalue.com.` to **`south-dotcom.github.io.`**. Use the hostname only, without `https://` or a path.
2. Replace the apex/root A record pointing to `51.15.191.141` with these four A records. In OVH, leave the subdomain field empty for the apex (`@`).

| OVH subdomain | Type | Target |
| --- | --- | --- |
| `www` | CNAME | `south-dotcom.github.io.` |
| *(empty)* | A | `185.199.108.153` |
| *(empty)* | A | `185.199.109.153` |
| *(empty)* | A | `185.199.110.153` |
| *(empty)* | A | `185.199.111.153` |

Keep existing MX, TXT, email-related, and unrelated records. Keep the existing nameservers. Change only conflicting web records for the root and `www`; do not use OVH's URL redirection feature for this migration. If the DNS zone now contains additional root or `www` A/AAAA records pointing to another web host, reconcile those before completing the cutover.

## Finish HTTPS

1. Allow DNS to propagate (up to 24 hours).
2. Open [GitHub Pages settings](https://github.com/south-dotcom/southvalue.com/settings/pages) and wait for the DNS check to pass.
3. Enable **Enforce HTTPS** once GitHub has issued the certificate. Certificate availability can also take time after DNS changes.
4. Check `https://www.southvalue.com/` and `https://southvalue.com/`. The apex should redirect to `www`.

The default `github.io` URL may redirect to the selected custom domain. Until DNS changes, it can therefore lead to the old site.

For rollback, restore the previous apex A record `51.15.191.141` and `www` CNAME `southvalue.com.`. Keep the previous web hosting available until the cutover is verified.

[Official GitHub custom-domain instructions](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)

[Official OVH DNS-zone instructions](https://docs.ovhcloud.com/en/guides/web-cloud/domains/dns-zone-edit)
