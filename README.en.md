# WebGhost – Traffic Imitator

**WebGhost** is a utility that generates a multi-page corporate website and simulates realistic traffic around it.

**Why is this needed:** To make it impossible to distinguish your HTTPS proxy traffic from regular corporate website traffic. WebGhost is suitable for masking proxy servers operating over HTTPS on port 443: NaiveProxy, Hysteria (with HTTP masking), Xray (VLESS+XTLS), Trojan, and others.

## ✨ What WebGhost Does

- **Generates a full-featured website** — blog, "About Us" section, contacts, sitemap, robots.txt, sitemap.xml, and all necessary resources (CSS, JS, images, fonts)
- **Simulates real visitors** — different browsers (Chrome, Firefox, Safari, Edge), reading pauses, link clicks, form submissions, UTM parameters
- **Adds background noise** — requests from search engine bots, security scanners, attempts to access non-existent pages
- **Supports mutual simulation** — two servers can generate traffic for each other
- **Updates with a single command** — `webghost update`

### 🎨 Unique Design for Each Domain

WebGhost does not create identical sites. During generation, the domain name is used as a source of entropy, so each domain receives:

- **Unique logo** — the company name in uppercase
- **Individual color scheme** — shades of accent color, gradients and transparencies vary slightly from site to site
- **Personalized contacts** — email and phone number are tied to your domain

Visually, all sites look consistent with a corporate style, but re-installing on a different domain will create a *similar but not identical* design. This makes it harder to create DPI signatures based on the site's appearance.

## 🚀 Quick Installation

```bash
wget https://github.com/krdn-dev/webghost/releases/latest/download/webghost-linux-amd64 -O /usr/local/bin/webghost
chmod +x /usr/local/bin/webghost
webghost --domain=example.com install
```
or
```bash
wget -O /usr/local/bin/webghost https://github.com/krdn-dev/webghost/releases/latest/download/webghost-linux-amd64 && chmod +x /usr/local/bin/webghost && webghost --domain=example.com install
```

## Step-by-Step Installation

### 1. Download the pre-built binary
```bash
wget https://github.com/krdn-dev/webghost/releases/latest/download/webghost-linux-amd64
```

### 2. Install it
```bash
sudo cp webghost-linux-amd64 /usr/local/bin/webghost
sudo chmod +x /usr/local/bin/webghost
```

### 3. Create the website and set up auto-start
```bash
webghost --domain=example.com install
```

## 📲 Main Commands

| Description | Command |
|-----------|----------------------|
| **Install website and configure systemd timer** | webghost --domain=example.com install |
| **Generate site structure only** | webghost --domain=example.com setup-site |
| **Manually run traffic simulation** | webghost --domain=example.com simulate |
| **Update WebGhost to the latest version** | webghost update |
| **Show all commands and examples** | webghost help или webghost |

## 📲 Usage Examples

### Full installation (site + auto-start)
```bash
webghost --domain=example.com install
```

### Generate site only
```bash
webghost --domain=example.com setup-site
```

### Run simulation
```bash
webghost --domain=example.com simulate
```

### Run simulation with file logging
```bash
webghost --domain=example.com --log=/var/log/webghost.log simulate
```

### Mutual simulation with another server
```bash
webghost --domain=example.com --remote=other-server.com simulate
```

### Mutual simulation with logging
```bash
webghost --domain=example.com --remote=other-server.com --log=/var/log/webghost.log simulate
```

### Verify everything is working
```bash
cat /var/log/webghost-activity.log
```
In the log, you will see page visits (HTTP 200), resource loading, User-Agent changes, and background noise.

## ❓ Frequently Asked Questions
### Can I learn more details about the signatures WebGhost uses to imitate traffic?
This information is intentionally not disclosed. The fewer details about internal algorithms and patterns become publicly available, the harder it is for Deep Packet Inspection (DPI) system developers to devise countermeasures. Developers on "the other side" also read documentation, so the most effective settings remain inside the code.

### Doesn't the DPI see that traffic originates from the local server rather than real visitors on the internet?
The ISP's DPI sees the overall data flow passing through the server. It analyzes metadata (IP, volume, timing) but cannot always determine the exact source of requests within the network. WebGhost mixes simulated traffic with real traffic, making it harder to isolate actual proxy connections against the general background. However, some advanced DPI systems can analyze latency or network flows (NetFlow) and notice that some requests originate from localhost. That is why we recommend using WebGhost in mutual simulation mode — where two servers exchange traffic, creating a bidirectional pattern that is significantly harder to distinguish from real traffic.

### How is traffic simulation distributed in different modes?
WebGhost supports two modes:

- **Local mode** — simulation of visitors only on your server. Used by default, creates a complete picture of an active site.

- **Mutual simulation mode** (`--remote`) — both servers exchange traffic. Your server creates a lightweight simulation for itself and a full simulation for the remote server. This makes traffic bidirectional and even more natural for DPI.

Exact parameters (number of sessions, burst probability) are not disclosed for security reasons — they are part of the DPI evasion strategy.

### How can I verify that WebGhost is working and traffic simulation is active?
All activity is logged to `/var/log/webghost-activity.log`.
To view the accumulated records, run the command in the terminal: `cat /var/log/webghost-activity.log`.

In the log, you will see: page visits (HTTP 200), resource requests (favicon.ico, style.css, images), User‑Agent changes and emulation of different browsers, background noise (rare requests to non-existent pages), and more.

### Does the remote server need to have WebGhost installed?
For maximum realism — yes. In mutual simulation mode, WebGhost requests the same pages that are generated by `setup-site`. If the remote server does not have such a site, it will respond with 404 errors, which is unnatural for a corporate portal and may attract DPI attention.

### Is a web server required for WebGhost to work?
Yes, a web server is mandatory. WebGhost creates the site and simulates traffic, but it does not handle incoming HTTPS requests by itself. A web server (Caddy, Nginx, etc.) must be installed and configured separately so that the site is accessible from the internet. If you are using [NaiveProxy Manager](https://github.com/krdn-dev/naiveproxy-manager), the web server is installed automatically.

## 🛠️ System Requirements
- Linux (amd64 или arm64)
- A domain pointed to the server (A record must resolve to your IP)
- Access to port 443 (HTTPS)
- Root privileges (for writing to `/var/www/html` and `/usr/local/bin`)
- A working web server answering via HTTPS on your domain (Caddy, Nginx и т.д.)

## 📄 License
This project is distributed under a proprietary license  [![License](https://img.shields.io/badge/License-Proprietary-red.svg)]()    
All rights reserved.

Permitted:    
Free use of the binary for personal and commercial purposes
Copying and redistribution of the binary while retaining authorship

Prohibited:    
Modification, decompilation, and reverse engineering
Publishing source code or its derivatives

## 💰 Support the Project  
If this script saved you time and you'd like to support its development, you can send a small donation    

[![Bitcoin](https://img.shields.io/badge/Bitcoin-F7931A?style=flat&logo=bitcoin&logoColor=white)](https://www.blockchain.com/explorer/addresses/btc/bc1p4ttkpfrgzpm7nyymyzdgyd2y6z04s62nxpygk38yylcp3t47m98qwnuhen)
```bc1p4ttkpfrgzpm7nyymyzdgyd2y6z04s62nxpygk38yylcp3t47m98qwnuhen```

❤️ Thank you for your support!     
⭐ Star this repository if the script helped you!

