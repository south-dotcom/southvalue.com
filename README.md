# South Value — GitHub Pages migration

The simplified site is in **`docs/`**. Edit `docs/index.html` to change the page, its inline CSS, or its small navigation script. Images and the two used font weights are self-hosted in `docs/assets/`. No build, package manager, framework, or external runtime is required.

The local `original/` folder (not published in this repository) preserves the public page and 42 assets downloaded from https://www.southvalue.com/ on 28 October 2026. It is an archive, not the publishing source. Some unused font and parallax URLs on the old server returned HTML instead of the expected asset; the new site uses only validated images and fonts. The original HTML includes a PHP redirect; GitHub Pages does not execute PHP, so it is omitted from the new page.

## What changed

- Replaced Bootstrap's layout with a small responsive CSS grid and flex layout.
- Moved all used CSS into the HTML; removed unused theme, form, social, carousel, animation, and icon-font rules.
- Removed jQuery, Bootstrap JS, WOW, easing, scrollTo, the preloader, unused email decoding, and animation dependencies.
- Kept a small inline script for the mobile menu, scrolled header, and active section. Native links, CSS smooth scrolling, and visible content work independently of the script.
- Preserved the mountain background, service icons, partner logos, Lato fonts, colors, section order, content, and desktop layout.
- Added accessible navigation labels, keyboard focus indicators, reduced-motion support, and clickable phone numbers.
- Preserved the original partner and career destinations. Their external content and currency have not been audited.

## Preview locally

From this folder, run:

```sh
python3 -m http.server 8000 --bind 127.0.0.1
```

Open http://127.0.0.1:8000/docs/ .

## Publication status — 28 October 2026

Repository: [south-dotcom/southvalue.com](https://github.com/south-dotcom/southvalue.com).

GitHub Pages is enabled from **main → /docs**. The first deployment succeeded and the live GitHub-address page was checked, including all images, fonts, navigation, and the absence of external CSS/JS libraries. **www.southvalue.com** is saved as the custom domain, and `docs/CNAME` matches it.

**Remaining: update the domain's OVH DNS, wait for GitHub's DNS check and certificate, then enable Enforce HTTPS in the repository's Pages settings.** Until DNS changes, the custom domain still serves the old host. The GitHub-address URL may redirect to that custom domain.

In OVH's DNS zone for `southvalue.com`, replace the current apex A record `51.15.191.141` with the four A records below. Change the current `www` CNAME target from `southvalue.com.` to `south-dotcom.github.io.`. For root A records, leave OVH's subdomain field empty. Keep nameservers `dns109.ovh.net` and `ns109.ovh.net`.

## DNS cutover

| Name | Type | Value |
| --- | --- | --- |
| `www` | CNAME | `south-dotcom.github.io` |
| `@` | A | `185.199.108.153` |
| `@` | A | `185.199.109.153` |
| `@` | A | `185.199.110.153` |
| `@` | A | `185.199.111.153` |

Replace only conflicting web-hosting records for these names. Preserve email (MX), verification TXT, and unrelated records. If existing apex AAAA records point to the previous web host, replace them with GitHub's documented IPv6 records or remove the obsolete web AAAA records. GitHub redirects the apex to `www` when both are correctly configured and `www` is the selected custom domain. DNS can take up to 24 hours to propagate.

Rollback: restore the previous web DNS records if needed; the archived source is retained in `original/`. DNS/account access is required for cutover and is not included in the website download.

Official instructions checked on 28 October 2026:

- [Create a GitHub Pages site](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site)
- [Configure a custom domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)

## Validation

Desktop layout and 390-pixel mobile layout were inspected in a browser. All eight inline images and the hero background load locally. The mobile menu opens and closes after selecting Contact; all section anchors have matching targets. There are no external stylesheets, scripts, or font requests. The first GitHub Pages deployment also passed browser checks; DNS cutover and custom-domain HTTPS remain pending.
