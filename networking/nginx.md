# NGINX on AWS EC2 with Custom Domain and HTTPS

This project demonstrates how to deploy an NGINX web server on an AWS EC2 instance and make it accessible through a custom domain managed with Cloudflare. HTTPS was enabled using Let's Encrypt and Certbot.

## Project Objectives

- Launch an AWS EC2 instance (Ubuntu)
- Install and configure NGINX
- Register and configure a custom domain
- Point the domain to the EC2 instance using Cloudflare DNS
- Secure the website with HTTPS using Let's Encrypt

## Technologies Used

- AWS EC2
- Ubuntu
- NGINX
- Cloudflare DNS
- Let's Encrypt
- Certbot

## Challenges Faced

While the EC2 instance and NGINX were working correctly over HTTP, the domain initially returned **Cloudflare Error 521 (Web Server Is Down)**.

### Root Cause

Cloudflare was configured to use HTTPS, but the origin server (NGINX) was only serving HTTP. As a result, Cloudflare could not establish a secure connection to the server.

### Solution

Installed Certbot and configured NGINX with a Let's Encrypt SSL certificate:

```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d your-domain.com -d subdomain.your-domain.com
```

After obtaining the SSL certificate, HTTPS worked successfully and the website became accessible through Cloudflare.

## Outcome

- ✅ NGINX deployed on AWS EC2
- ✅ Custom domain configured with Cloudflare
- ✅ HTTPS enabled with Let's Encrypt
- ✅ Website successfully accessible over HTTPS

## Live Demo

https://nginx.ina-warsame.uk