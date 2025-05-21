Then visit:
curl http://YOUR.SERVER.IP:8080


📘 NGINX Configuration Notes

🔧 1. Main Config File
- Default Location: /etc/nginx/nginx.conf
- Purpose: It's the entry point of the NGINX configuration.
- Structure: NGINX uses a block-based structure:
    - events { ... }
    - http { ... }

🌐 2. http Block
- Purpose: The main block for defining HTTP-related settings and servers.
- What You Can Do:
    - Set global directives (e.g., logging, MIME types).
    - Define multiple server blocks (virtual hosts).

  Example:
  http {
  include mime.types;
  default_type application/octet-stream;

      server {
          ...
      }
  }

🖥️ 3. server Block (Virtual Host)
- Purpose: Defines how NGINX responds to requests on a particular port and hostname.
- Key Directives:
    - listen: Specifies the port NGINX listens on (e.g., 80, 443).
    - server_name: Specifies the hostname to respond to (e.g., example.com).
    - root: The directory containing website files.
    - index: The default file to serve when accessing a directory.

  Example:
  server {
  listen 80;
  server_name example.com;
  root /var/www/html;
  index index.html;
  }

📁 4. location Block
- Purpose: Defines how to handle requests for specific URLs or paths.
- Examples:
    - Serve files
    - Redirect or rewrite URLs
    - Proxy requests to other servers

  Example:
  location / {
  try_files $uri $uri/ =404;
  }

  Explanation:
    - try_files $uri $uri/ =404;: Tries to serve the requested file or directory, returning a 404 if not found.

📄 5. MIME Types
- Purpose: Defined by include mime.types;
- Function: Helps NGINX send the correct Content-Type headers for files like .html, .css, .jpg, etc.

📜 6. Logging
- NGINX Logging: Supports access and error logs.
    - Access log: /var/log/nginx/access.log
    - Error log: /var/log/nginx/error.log

  Usage: Logs are used for debugging, monitoring, and security auditing.

🔄 7. Reloading Config
- After Editing: Reload NGINX to apply changes:
    - Reload: sudo systemctl reload nginx
    - Restart (if necessary): sudo systemctl restart nginx

🛠️ 8. Testing Configuration
- Best Practice: Test the configuration before reloading or restarting NGINX.
    - Command to test: sudo nginx -t

  Note: Testing the config helps prevent issues in production.

🔐 9. Security Tips
- SSL for HTTPS: Use listen 443 ssl; for secure HTTPS connections.
- Prevent Directory Listing: Use autoindex off;.
- Restrict IPs: Use deny and allow in location blocks to control access.

📦 10. Useful NGINX Modules (Built-in)
- gzip: For compressing responses.
- proxy_pass: For reverse proxying.
- rewrite: For URL rewrites.
- expires: For setting caching headers.

🧠 Extra Learning
- Virtual Hosts: You can host multiple websites on one NGINX instance by defining multiple server blocks.
- Reverse Proxy: NGINX can forward requests to a backend server (e.g., Node.js, Python).
- Load Balancing: Distribute traffic across multiple backend servers for improved performance and reliability.
