# Personal Website — Design & Deployment Spec

## Overview

A single-page personal portfolio site for Brad, a CS student at Texas A&M-RELLIS.
Deployed as a static HTML site on an AWS EC2 instance served via Nginx.

---

## Site Structure

```
/
├── index.html          # Main single-page site
├── resume.pdf          # Linked from the hero CTA (add manually)
└── assets/             # Optional: images, favicon, etc.
```

---

## Design System

### Typography
| Role | Font | Source |
|---|---|---|
| Display / Hero headline | DM Serif Display | Google Fonts |
| Monospace labels, nav, tags, buttons | Space Mono | Google Fonts |
| Body / prose | system-ui (sans-serif) | System |

### Color Palette

All colors are defined as CSS custom properties in `:root` with full dark mode support via `@media (prefers-color-scheme: dark)`.

| Token | Light | Dark | Usage |
|---|---|---|---|
| `--bg` | `#f9f8f6` | `#1a1a18` | Page background |
| `--surface` | `#ffffff` | `#242422` | Cards, nav |
| `--text` | `#1a1a18` | `#f0ede8` | Primary text |
| `--muted` | `#6b6b67` | `#a0a09b` | Secondary text |
| `--hint` | `#a0a09b` | `#6b6b67` | Labels, footer |
| `--blue-mid` | `#185FA5` | `#85b7eb` | Links, featured border, CTA |
| `--blue-bg` | `#E6F1FB` | `#0c447c` | Blue tag background |
| `--green-text/bg` | `#3B6D11` / `#EAF3DE` | `#97C459` / `#173404` | Green project tag |
| `--amber-text/bg` | `#854F0B` / `#FAEEDA` | `#FAC775` / `#412402` | Amber project tag |

### Spacing & Radius
- Border radius cards: `12px` (`--radius-lg`)
- Border radius buttons/tags: `8px` (`--radius-md`)
- Section padding: `2.5rem 0`
- Max content width: `900px`, centered

---

## Sections

### Nav
- Left: `Brad · CS @ TAMU-RELLIS` in Space Mono
- Right: anchor links → `#projects`, `#skills`, `#contact`

### Hero
- IBM Qiskit Advocate badge (blue pill)
- Headline: *"Building at the edge of software."* — DM Serif Display
- Subheading: brief bio mentioning ML, quantum computing, networking, healthcare software
- CTAs: `View Projects` (primary blue button) + `Download Résumé` (secondary outline button linking to `/resume.pdf`)

### Projects
Three cards in a responsive auto-fit grid (`minmax(250px, 1fr)`):

| Project | Tag Color | Stack | Featured |
|---|---|---|---|
| ClinicScheduler | Blue | .NET MAUI, Blazor | Yes (1.5px blue border) |
| Patient Onboarding | Amber | Django, AWS Lambda, DynamoDB, Rekognition | No |
| Network Intrusion Detection | Green | PyTorch, NSL-KDD, PCA+SVM | No |

### Skills
Pill-shaped tags in a flex-wrap row:
`Python`, `Django`, `Qiskit`, `PyTorch`, `C# / .NET`, `AWS Lambda`, `DynamoDB`, `Rekognition`, `Cisco IOS`, `CCNA-level`, `Linux`, `SVM / MLP`

### Contact
Three monospace anchor links: GitHub, LinkedIn, Email

### Footer
`© 2026 Brad — hosted on EC2 — built with curiosity`

---

## TODOs Before Deploying

- [ ] Replace `href="https://github.com/"` placeholders with real GitHub repo URLs
- [ ] Replace `href="https://linkedin.com/in/"` with real LinkedIn URL
- [ ] Replace `href="mailto:you@example.com"` with real email address
- [ ] Add `resume.pdf` to the server root
- [ ] Add a `<link rel="icon">` favicon
- [ ] (Optional) Add an About section with a photo

---

## EC2 Deployment — Nginx

### 1. Install Nginx
```bash
sudo apt update && sudo apt install nginx -y
```

### 2. Copy site files
```bash
sudo cp index.html /var/www/html/
# If you add a resume: sudo cp resume.pdf /var/www/html/
```

### 3. Nginx server block (`/etc/nginx/sites-available/default`)
```nginx
server {
    listen 80;
    server_name YOUR_EC2_PUBLIC_IP_OR_DOMAIN;

    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

### 4. Enable and restart
```bash
sudo nginx -t
sudo systemctl restart nginx
```

### 5. (Optional) HTTPS with Let's Encrypt
```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d yourdomain.com
```

> Requires a domain name pointed at your EC2 Elastic IP.

---

## Potential Enhancements for Kiro

- Add an **About** section with a short bio paragraph and optional headshot
- Add **project detail pages** (`/projects/clinicscheduler.html`, etc.) with architecture diagrams
- Wire up a **contact form** using AWS SES or a simple Lambda POST endpoint
- Add a **dark/light mode toggle** button in the nav
- Add **smooth scroll reveal** animations using the Intersection Observer API
- Add a **blog** section using markdown files rendered via a lightweight JS parser
- Consider migrating to a **static site generator** (Eleventy, Hugo) for easier content management as the site grows
