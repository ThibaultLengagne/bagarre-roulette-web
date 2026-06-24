# roulette-web

A lightweight, self-hosted spinning wheel you can customize with a plain text file. Pick a random winner for game nights, team assignments, giveaways, or anything else that needs a fair spin.

Hosted at **https://roulette.thibaultlengagne.com** via AWS S3 + CloudFront.

![Roulette Wheel](https://img.shields.io/badge/python-3.8+-blue) ![License](https://img.shields.io/badge/license-MIT-green)

## Features

- **Canvas-drawn wheel** with smooth spin animation and a fixed pointer at the top
- **Text-file configuration** — edit `segments.txt`, reload, and spin again
- **No build step** — vanilla HTML, CSS, and JavaScript
- **Comment support** in `segments.txt` (lines starting with `#` are ignored)
- **Dark UI** with segment preview and one-click reload
- **Terraform** — S3 static hosting, CloudFront CDN, ACM TLS, Route 53 records

## Local development

1. Clone the repository:

   ```bash
   git clone git@github.com:ThibaultLengagne/bagarre-roulette-web.git
   cd bagarre-roulette-web
   ```

2. Edit `segments.txt` — one option per line:

   ```text
   Alice
   Bob
   Charlie
   # Lines starting with # are ignored
   ```

3. Start the local server:

   ```bash
   python3 server.py
   ```

4. Open [http://localhost:8000](http://localhost:8000) in your browser.

> A local server is required because the app loads `segments.txt` over HTTP. Opening `index.html` directly in the browser (`file://`) will not work.

## Deploy to AWS

Infrastructure lives in `infra/` and provisions:

| Resource | Purpose |
|----------|---------|
| S3 bucket | Stores `index.html` and `segments.txt` (private, CloudFront-only) |
| CloudFront | HTTPS CDN at `roulette.thibaultlengagne.com` |
| ACM certificate | TLS cert (DNS-validated, `us-east-1`) |
| Route 53 records | `A`/`AAAA` alias to CloudFront + ACM validation |

### Prerequisites

- [Terraform](https://www.terraform.io/) >= 1.5
- AWS CLI configured (`aws configure` or env vars)
- An existing Route 53 hosted zone for `thibaultlengagne.com`

### First-time setup

```bash
cd infra
terraform init
```

Import your existing DNS zone **before** the first `apply`:

```bash
# Find the zone ID in the Route 53 console or with:
aws route53 list-hosted-zones-by-name --dns-name thibaultlengagne.com

terraform import aws_route53_zone.main <ZONE_ID>
```

Deploy:

```bash
terraform apply
```

Terraform uploads `index.html` and `segments.txt` to S3. After apply, the site is available at **https://roulette.thibaultlengagne.com**.

### Updating content

Edit `segments.txt` or `index.html`, then:

```bash
cd infra
terraform apply
```

`segments.txt` is served with `Cache-Control: no-store` so the in-app **Reload from file** button works without invalidating CloudFront.

### Configuration

Copy `infra/terraform.tfvars.example` to `infra/terraform.tfvars` to override defaults:

| Variable | Default |
|----------|---------|
| `domain_name` | `roulette.thibaultlengagne.com` |
| `zone_name` | `thibaultlengagne.com` |
| `aws_region` | `eu-west-3` |
| `bucket_name` | auto-generated |

## Project structure

```
roulette-web/
├── index.html              # Wheel UI and spin logic
├── segments.txt            # Wheel segments
├── server.py               # Local dev server
├── infra/                  # Terraform (S3, CloudFront, ACM, Route 53)
│   ├── main.tf
│   ├── s3.tf
│   ├── cloudfront.tf
│   ├── acm.tf
│   ├── route53.tf
│   └── ...
└── README.md
```

## Requirements

- Python 3.8+ for local dev (stdlib only)
- Terraform + AWS account for hosting
- Any modern browser with Canvas support

## License

MIT — use it however you like.
