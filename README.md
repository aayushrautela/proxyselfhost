# Self-Hosting Without a Public IP: Step-by-Step Guide

### Introduction
Self-hosting your services at home offers several advantages. Your home PC can be more secure, more powerful, and customizable to your needs compared to shared hosting environments. However, not everyone has a public IP address (or only get a public ipv6, like myself), which is necessary for direct access to your home-hosted services. This guide walks you through setting up a secure tunnel using a VPS (Virtual Private Server) to allow access to your home-hosted services, even without a public IP.

---

## Prerequisites
1. A VPS with a public IP (Oracle's Always Free tier or Hetzner's cheapest options are good choices).
2. A domain name or subdomain (DuckDNS for free options).
3. A basic understanding of NGINX or any proxy server.
4. Basic knowledge of WireGuard VPN.

---

### [Part 1: Initial Setup](initial_setup.md)
### [Part 2: Home Server Setup](home_server_config.md)
### [Part 3: Setting up host forwarding](NGINX_config.md)
### [Part 4: Setting up SSL encryption](setting_ssl.md)
