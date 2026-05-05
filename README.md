# SASI.ASIA Portfolio

Standalone portfolio project for `sasi.asia`.

## Project Purpose

This is the public-facing strategic advisor website for Sasion Nanthaphiriyakit. It presents:

- brand and positioning
- advisory services
- case studies
- insight content
- contact paths

## Main Pages

- `home.html` — landing page
- `services.html` — advisory services
- `case-studies.html` — selected work
- `insights.html` — editorial insights and Market Intelligence Radar
- `contact.html` — inquiry and contact page

## Notes

- `insights.html` includes the live `Market Intelligence Radar` section
- news data is loaded from `/api/news` through the backend proxy, not directly from the browser
- this folder is intentionally separate in content scope from the crypto dashboard and bot internals, even though production nginx serves them under the same domain
