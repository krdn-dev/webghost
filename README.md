# WebGhost-Traffic Imitator

**Realistic corporate website generator & traffic camouflage for privacy obfuscation.**

Creates unique, fully-functional enterprise websites with dynamic content, SSL certificates, blog posts, legal pages, contact forms, and human-like browsing patterns — all from a single binary.

---

## Quick Start

```bash
# Install (generates site + sets up automatic traffic simulation)
webghost --domain=example.com install

# Or just generate the website
webghost --domain=example.com setup-site

# Run traffic simulation manually
webghost --domain=example.com simulate

# Mutual imitation with a remote server
webghost --domain=example.com --remote=partner.com simulate

Features
🎨 Unique Websites for Every Domain
24+ color themes with micro-variations

Generated company names (1000+ combinations)

Unique logos (SVG with domain initials)

Cookie consent banners (3 styles)

Dynamic taglines, office locations, contact info

Blog posts with varied titles and content

Privacy Policy, Terms of Service, Cookie Policy pages

security.txt, humans.txt, robots.txt, sitemap.xml

🧠 Smart Traffic Simulation
Realistic User-Agent rotation (Chrome, Firefox, Safari, Edge, Mobile)

Organic browsing patterns: reading, quick tabs, back button, page refresh

UTM-tagged visits from search engines and social media

Form submissions with random data

Video downloads

Bot traffic (Googlebot, Bingbot, YandexBot, DuckDuckBot)

Scanner noise (SQL injection attempts, path traversal)

Background activity: ICMP ping, DNS lookup, health checks, CORS preflight

🛡️ Designed for Obfuscation
HTTPS traffic indistinguishable from real corporate websites

Works with NaiveProxy, V2Ray, Xray, and other tunneling solutions

Systemd timer for automated periodic traffic generation

Log rotation included

Installation
Download Binary
curl -L -o /usr/local/bin/webghost https://github.com/krdn-dev/webghost/releases/latest/download/webghost-linux-amd64
chmod +x /usr/local/bin/webghost

Or Build from Source
git clone https://github.com/krdn-dev/webghost.git
cd webghost
go build -trimpath -ldflags="-s -w" -o webghost .

Commands
Command	Description
install	Generate website + install systemd timer for automatic traffic
setup-site	Generate website structure only
simulate	Run traffic simulation once
update	Self-update from GitHub Releases

Options
Option	Description
--domain=<domain>	Domain name (required)
--remote=<host>	Remote server for mutual traffic imitation
--log=<path>	Log file path (default: stdout)

Systemd Integration
When installed via webghost install, a systemd timer runs traffic simulation automatically:

Every 45 minutes during daytime (06:00–22:59)

Every 2 hours at night (23:00–05:59)

Randomized delay of up to 20 minutes to avoid patterns

Logs are written to /var/log/webghost-activity.log with monthly rotation.

Example Log Output
2026/05/14 00:32:15 Starting 4 sessions on https://example.com (mode=local)
[Session 1] 2026/05/14 00:32:22 User: Mozilla/5.0 ... Chrome/128.0.0.0 https://example.com/blog/
[Session 1] 2026/05/14 00:32:22 HTTP 200 - https://example.com/blog/
2026/05/14 00:32:22 Health check HTTP 200 - HEAD https://example.com
2026/05/14 00:32:22 DNS lookup example.com: 93.184.216.34
2026/05/14 00:32:24 Ping example.com: 3 packets transmitted, 3 received
[Session 2] 2026/05/14 00:32:38 Bot: Googlebot/2.1 https://example.com/robots.txt

License
MIT License — see LICENSE file for details.

Related Projects
NaiveProxy Manager — One-command NaiveProxy installation with WebGhost integration.
