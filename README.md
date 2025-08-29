NGINX Reverse Proxy with SSL and Load Balancing

This repository contains an NGINX configuration that sets up a reverse proxy with SSL termination, HTTP → HTTPS redirection, load balancing across multiple Node.js instances, and an optional local test server.

🔹 Features

Load Balancing: Distributes traffic across multiple Node.js servers (3001, 3002, 3003) using the least_conn method.

SSL Termination: Handles HTTPS traffic using a self-signed or trusted SSL certificate.

HTTP to HTTPS Redirect: Automatically redirects all HTTP traffic (port 80) to HTTPS (port 443).

Optional Local Test Server: Provides a non-SSL endpoint on port 8080 for debugging.

Proxy Headers: Preserves client information with X-Real-IP and X-Forwarded-* headers.

🔹 Prerequisites

Installed NGINX (check with nginx -v).

SSL certificate and key (self-signed or from a trusted CA). Place them in:

/Users/nana/nginx-cert/nginx.crt
/Users/nana/nginx-cert/nginx.key


Running Node.js backend servers on ports 3001, 3002, and 3003.

🔹 Configuration File

Path: /etc/nginx/nginx.conf (or inside /etc/nginx/conf.d/ depending on setup).

Key Sections:

Upstream cluster:

upstream nodejs_cluster {
    least_conn;
    server 127.0.0.1:3001;
    server 127.0.0.1:3002;
    server 127.0.0.1:3003;
}


HTTPS server with SSL termination (listen 443 ssl;).

HTTP → HTTPS redirect server (listen 80;).

Optional non-SSL proxy on port 8080.

🔹 Usage
1. Test NGINX config:
nginx -t

2. Reload NGINX:
sudo systemctl reload nginx

3. Access the application:

Secure (recommended): https://localhost

Non-secure redirect: http://localhost
 → redirects to HTTPS

Local test server: http://localhost:8080

🔹 Troubleshooting

SSL errors: Ensure your certificate and key paths are correct.

502 Bad Gateway: Check if Node.js servers are running on ports 3001, 3002, 3003.

Permission errors: Ensure NGINX has access to certificate files.
