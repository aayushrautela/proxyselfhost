## Part 3: Configure NGINX Proxy Manager on VPS

1. In **NGINX Proxy Manager**, add a new proxy host entry pointing to the private IP of the home service you want to expose.
   - Use the domain name you set up earlier.
   - Select HTTP as the scheme, as WireGuard is already encrypting the traffic.
   - This allows you to securely access your home services using the domain name through the WireGuard tunnel.

2. **Test the setup**: You should now be able to access your home-hosted service through your domain, though you'll receive a "Not Secure" warning due to the lack of SSL at this point.
