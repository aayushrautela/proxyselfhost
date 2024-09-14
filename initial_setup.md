
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

### [Part 2: Home Server Setup](home_server_config.md)
