# Actual Budget Add-on Documentation

## Important: HTTPS Required

Actual Budget requires **SharedArrayBuffer** to function, which browsers only enable over secure connections. You **must** access Home Assistant via HTTPS for this add-on to work properly.

If you see "Initializing the connection to the local database" stuck or "SharedArrayBuffer is not defined" errors, you need to set up HTTPS.

## Setting Up HTTPS

### Option 1: NGINX Proxy Manager (Recommended)

1. **Install NGINX Proxy Manager add-on**
   - Go to Settings → Add-ons → Add-on Store
   - Search for "NGINX Proxy Manager" or add the repository: `https://github.com/hassio-addons/addon-nginx-proxy-manager`
   - Install and start the add-on

2. **Get a domain** (if you don't have one)
   - Use [DuckDNS](https://www.duckdns.org/) for a free subdomain (e.g., `yourhome.duckdns.org`)
   - Point it to your public IP address

3. **Configure NGINX Proxy Manager**
   - Access the NGINX Proxy Manager web UI
   - Add a new Proxy Host:
     - **Domain**: Your domain (e.g., `yourhome.duckdns.org`)
     - **Forward Hostname/IP**: `homeassistant` or `172.30.32.1`
     - **Forward Port**: `8123`
   - In the SSL tab:
     - Request a new SSL certificate with Let's Encrypt
     - Enable "Force SSL"
   - Enable "Websockets Support"

4. **Configure Home Assistant**
   - Add to your `configuration.yaml`:
     ```yaml
     http:
       use_x_forwarded_for: true
       trusted_proxies:
         - 172.30.33.0/24
     ```
   - Restart Home Assistant

5. **Access via HTTPS**
   - Navigate to `https://yourdomain.com`
   - Open Actual Budget via the sidebar or "Open Web UI"

### Option 2: DuckDNS + Let's Encrypt Add-on

1. **Set up DuckDNS**
   - Create a free account at [duckdns.org](https://www.duckdns.org/)
   - Create a subdomain and note your token

2. **Install the DuckDNS add-on**
   - Go to Settings → Add-ons → Add-on Store
   - Install "DuckDNS"
   - Configure with your domain and token
   - Enable "Accept Let's Encrypt terms"

3. **Configure Home Assistant**
   - Add to your `configuration.yaml`:
     ```yaml
     http:
       ssl_certificate: /ssl/fullchain.pem
       ssl_key: /ssl/privkey.pem
     ```
   - Restart Home Assistant

4. **Access via HTTPS**
   - Navigate to `https://yourdomain.duckdns.org:8123`

### Option 3: Cloudflare Tunnel (No Port Forwarding)

If you can't open ports on your router, use Cloudflare Tunnel:

1. **Install Cloudflared add-on**
   - Add repository: `https://github.com/brenner-tobias/addon-cloudflared`
   - Install "Cloudflared"

2. **Set up Cloudflare**
   - Create a free Cloudflare account
   - Add your domain to Cloudflare
   - Follow the add-on documentation to create a tunnel

3. **Access via HTTPS**
   - Your Home Assistant will be available at your configured domain

### Option 4: Nabu Casa (Easiest)

[Nabu Casa](https://www.nabucasa.com/) (Home Assistant Cloud) provides automatic HTTPS with no configuration:

1. Subscribe to Nabu Casa
2. Enable Remote Access in Home Assistant
3. Access via your `*.ui.nabu.casa` URL

## Accessing Actual Budget

Once HTTPS is configured:

1. **Via Ingress (Recommended)**: Click "Open Web UI" on the add-on page or use the sidebar
2. **Direct Access**: Navigate to `https://yourdomain.com:5006` (if port is exposed)

## Data Storage

Your budget data is stored persistently in `/data` and survives add-on updates and restarts.

## Troubleshooting

### "Initializing the connection to the local database" stuck

This means SharedArrayBuffer is blocked. Ensure you're accessing Home Assistant via HTTPS, not HTTP.

### Blank screen or 502 errors

Check the add-on logs (Settings → Add-ons → Actual Budget → Log) for errors. Ensure the add-on has fully started.

### Font loading errors (404)

Clear your browser cache and reload. Some assets may be cached with incorrect paths.

## More Information

- [Actual Budget Documentation](https://actualbudget.org/docs/)
- [Actual Budget GitHub](https://github.com/actualbudget/actual)
- [Home Assistant HTTPS Guide](https://www.home-assistant.io/docs/configuration/remote/)
