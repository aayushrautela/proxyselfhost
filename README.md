# Self-Hosting Without a Public IP: Step-by-Step Guide

### Introduction
Self-hosting your services at home offers several advantages. Your home PC can be more secure, more powerful, and customizable to your needs compared to shared hosting environments. However, not everyone has a public IP address, which is necessary for direct access to your home-hosted services. This guide walks you through setting up a secure tunnel using a VPS (Virtual Private Server) to allow access to your home-hosted services, even without a public IP.

---

## Prerequisites
1. A VPS with a public IP (Oracle's Always Free tier or Hetzner's cheapest options are good choices).
2. A domain name or subdomain (DuckDNS for free options).
3. A basic understanding of NGINX or any proxy server.
4. Basic knowledge of WireGuard VPN.

---

## Step 1: Get a VPS with Public IP
To expose your home services, you'll first need a VPS with a public IP. I recommend Oracle's Always Free tier or a cheap option from Hetzner, but feel free to use any VPS provider.

Why host at home?
- **Power**: Your home PC can be more powerful than a VPS.
- **Security**: Hosting on your own hardware allows you to have full control over security.

---

## Step 2: Install a Proxy Server on the VPS
Set up a proxy server on your cloud VPS. I personally recommend **NGINX** for its performance and ease of use, but any proxy server will do. Using a proxy server with a GUI (like **NGINX Proxy Manager**) is highly recommended for easier management, but you can also edit the configuration files manually.

---

## Step 3: Get a Domain Name
You'll need a domain name to access your home-hosted services. **DuckDNS** offers 5 free subdomains, which is a great place to start. You can upgrade to a paid domain later.

- **DuckDNS**: Set up a free subdomain and point it to the public IP of your cloud server.
- **Paid Domains**: Optionally, register a custom domain and configure DNS settings to point to your VPS.

---

## Step 4: Install WireGuard
You need a VPN tunnel to securely route traffic between your home network and the VPS. WireGuard is a modern, fast, and lightweight VPN protocol that is much simpler to configure compared to OpenVPN, making it an ideal choice.

---

## Step 5: Configure WireGuard on the VPS

1. **Generate a key-pair for the VPS:**
   `
   wg genkey | tee server.key | wg pubkey > server.pub
   `

2. **Create a WireGuard configuration file (`wg0.conf`):**
   `
   sudo nano /etc/wireguard/wg0.conf
   `

3. **Add interface details (address, listen port):**
   `
   [Interface]
   Address = 10.100.0.1/24  # Change this to your desired subnet
   ListenPort = 47111
   `

4. **Add the server’s private key to the config:**
   `
   echo "PrivateKey = $(cat server.key)" >> /etc/wireguard/wg0.conf
   `

5. **Open port 47111**: Make sure port 47111 is open in the VPS’s firewall.

6. **Set up IP forwarding and routing**:
   - Use iptables to allow traffic forwarding between the real network and the WireGuard tunnel.
   - Add a routing entry for sending traffic from your VPS over the WireGuard interface (`wg0`).

7. **Enable and start WireGuard:**
   `
   sudo systemctl enable wg-quick@wg0.service
   sudo systemctl daemon-reload
   sudo systemctl start wg-quick@wg0
   `

---

## Step 6: (Optional) Install NGINX Proxy Manager
While not required, **NGINX Proxy Manager** offers a GUI for easy configuration of your reverse proxy settings, making management of domains and SSL certificates simple.

---

## Part 2: Setting Up the Home Server
Now that your VPS is set up, configure your home server to connect to the VPN.

1. **Install WireGuard on your home server** and connect it to the VPS's WireGuard instance.
2. **Ensure services you want to expose** (e.g., web server, NAS) are running on your home server.
3. **Test the connection** by pinging the home server from the VPS to ensure it's reachable.

---

## Part 3: Configure NGINX Proxy Manager on VPS

1. In **NGINX Proxy Manager**, add a new proxy host entry pointing to the private IP of the home service you want to expose.
   - Use the domain name you set up earlier.
   - Select HTTP as the scheme, as WireGuard is already encrypting the traffic.
   - This allows you to securely access your home services using the domain name through the WireGuard tunnel.

2. **Test the setup**: You should now be able to access your home-hosted service through your domain, though you'll receive a "Not Secure" warning due to the lack of SSL at this point.

---

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
