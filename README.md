# Self-Hosting Without a Public IP: Step-by-Step Guide

### Introduction
Self-hosting your services at home offers several advantages. Your home PC can be more secure, more powerful, and customizable to your needs compared to shared hosting environments. However, not everyone has a public IP address, which is necessary for direct access to your home-hosted services. This guide walks you through setting up a secure tunnel using a VPS (Virtual Private Server) to allow access to your home-hosted services, even without a public IP.

---

## Prerequisites
1. A VPS with a public IP (Oracle's Always Free tier or Hetzner's cheapest options are good choices).
2. A domain name or subdomain (DuckDNS for free options).
3. A basic understanding of NGINX or any proxy server.
4. Basic knowledge of WireGuard VPN.



## Part 4: Set Up SSL Certificates
To avoid insecure connection warnings, you'll need an SSL certificate.

The **general idea** behind SSL certificates is to secure the communication between your browser and the server, ensuring that data is encrypted and trusted. SSL also removes the "Not Secure" browser warning when accessing your services.

### Steps to set up SSL:

1. **For DuckDNS users**, you can use the DNS challenge method provided by Let's Encrypt.
   - Go to the DuckDNS website and get the token associated with your domain.
   - Use this token to issue an SSL certificate through **Let's Encrypt** by leveraging a DNS challenge.
   - Tools like **Certbot** or built-in functionality in NGINX Proxy Manager can help automate this process.

2. **Update NGINX Proxy Manager** with the SSL certificate for secure access to your services over HTTPS:
   - In NGINX Proxy Manager, go to the "SSL" tab of your proxy host entry and select the SSL certificate you’ve issued.
   - Enable force SSL to ensure that all traffic to your domain is encrypted.

3. If you're using a custom domain, the steps will vary slightly depending on your DNS provider. Most modern domain registrars support DNS challenges through Let's Encrypt or allow you to manually install SSL certificates on your VPS.

---

## Conclusion
You're now all set! With this setup, you can securely access your home-hosted services using your domain, tunneled through WireGuard, without needing a public IP at home.

---

### Notes
- This setup is highly customizable based on your specific needs and can be adapted to different VPNs, proxy servers, or domain configurations.
- Feel free to upgrade your domain or VPS as necessary as your needs grow.
```
