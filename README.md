# cmp-blocklist

> Nordic media CMP, paywall and tracker DNS blocklist for Pi-hole and AdGuard Home.

Most ad and tracker blocklists miss the Consent Management Platform (CMP) domains that Nordic news sites use to serve cookie walls and GDPR consent dialogs. This repository targets those directly: the per-publisher and per-vendor CMP endpoints used across Swedish, Danish, Norwegian, Finnish and Icelandic media, plus paywall systems, media-specific trackers, and the major global CMP vendors.

Blocking the CMP endpoint at the DNS level suppresses the consent dialog so article content loads without the overlay, rather than storing a "consent" decision on your behalf.

## What's in here

| File | Purpose | Use with |
|------|---------|----------|
| `cmp-blocklist.txt` | The main list: Nordic media CMP, paywall and tracker domains, plus global CMP vendors | Pi-hole, AdGuard Home (DNS) |
| `cmp-blocklist-ublock.txt` | Companion cosmetic rules for Aller Media SE (self-hosted Didomi) that DNS cannot block | uBlock Origin only |
| `additional_blocklists.md` | A curated index of complementary general-purpose blocklists (ads, malware, phishing, trackers, native-app telemetry, anti-bypass) | reference |

## What it blocks

- Consent Management Platforms for the main Nordic media groups: Schibsted, Bonnier News, NTM, Stampen, Alma Media, Amedia, Aller, and others.
- Global CMP vendors: OneTrust, Didomi, Usercentrics / Cookiebot, Sourcepoint, TrustArc, Quantcast Choice, Cookie Information, Consentmanager, Iubenda, Osano, Axeptio, CookieFirst, and more.
- IAB TCF shared consent infrastructure.
- Paywall systems: Piano / TinyPass, Tulo.
- Media trackers: Cxense (Piano Analytics), Permutive, Chartbeat, Parse.ly, ComScore, Nielsen.
- Ad networks: Google Ad Manager, Xandr, Magnite, PubMatic, Criteo, Teads. See the warning below, blocking these breaks ad-funded layouts.

## Quick start

Subscribe to the raw URL of the main list:

```
https://raw.githubusercontent.com/techsurrusr/cmp-blocklist/main/cmp-blocklist.txt
```

**Pi-hole:** Lists (Adlists), paste the URL, Add, then update gravity (`pihole -g`, or Tools, Update Gravity).

**AdGuard Home:** Filters, DNS blocklists, Add blocklist, Add a custom list, paste the URL.

**uBlock Origin (for the cosmetic companion only):** Dashboard, Filter lists, Import, paste:

```
https://raw.githubusercontent.com/techsurrusr/cmp-blocklist/main/cmp-blocklist-ublock.txt
```

## Important warnings

- **Subscriber login (SSO).** Entries marked `[SSO]` in the file also break subscriber login for the publisher. Comment them out if you hold an active subscription.
- **Ad-funded layouts.** The "Ad networks" section breaks sites whose layout depends on ad slots. Remove that section if you want consent suppression without touching ads.
- **Do not block the rendering dependencies.** The file header lists domains that are deliberately left unblocked because article JavaScript waits on them before rendering body content (for example `static.privacy.schibsted.com`). Blocking those makes articles hang. Leave them alone.
- **Hard cookie walls.** This suppresses the consent dialog, it does not consent for you. A minority of sites gate content behind a hard wall and may still refuse to load. That is expected.
- **Cosmetic rules are uBO-only.** `cmp-blocklist-ublock.txt` holds uBlock Origin cosmetic filters. Pi-hole and AdGuard Home ignore them, so do not add that file as a DNS list.

## additional_blocklists.md

A curated index of complementary general-purpose lists you can run alongside this one (ads, malware, phishing, first-party trackers, native-app telemetry, and anti-bypass). It is a reference table, not a subscribable list.

One note: the hagezi "Encrypted DNS/VPN/Proxy Bypass" entry blocks the public DoH, VPN, Tor and proxy endpoints devices use to route around your DNS filtering. It can break your own VPN, Tor or custom DoH resolver, so allowlist what you use before enabling it.

## Format

- Sink: `127.0.0.1 <domain>`, standard hosts syntax.
- Inline `#` comments annotate each entry.
- Grouped by country and by vendor, with a changelog and version in the file header.

## Maintenance

Entries are live-tested against the target sites (browser consent-flow and article-render checks). The current version and verification date are in the header of `cmp-blocklist.txt`. Lists drift as publishers switch CMP vendors, so re-verification happens periodically.

## Contributing

Issues and pull requests are welcome: a broken suppression, a site that no longer loads, a publisher that migrated to a new CMP, or a domain that should be added or removed.

## License

No license is set yet. Add a `LICENSE` file to make reuse terms explicit. For a blocklist, a permissive choice such as MIT or a public-domain dedication (CC0) is common; pick the one that fits your intent.

## Disclaimer

Provided as-is, with no warranty. Blocking consent, paywall and ad domains can change or break site behaviour. Use at your own discretion, and allowlist anything you need.
