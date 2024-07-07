FROM nginx:1.27.0

# Install the GNU Compiler Collection
RUN apt-get update && apt-get install -y \
        gcc mono-mcs \
    && rm -rf /var/lib/apt/lists/* \
    && rm /etc/nginx/conf.d/default.conf \
    # Create the necessary directories
    && mkdir -p /var/cache/nginx/client_temp \
        /var/cache/nginx/proxy_temp \
        /var/cache/nginx/fastcgi_temp \
        /var/cache/nginx/scgi_temp \
        /var/cache/nginx/uwsgi_temp \
        /etc/nginx/sites-available \
        /etc/nginx/sites-enabled

# Copy the NGINX configuration files
COPY blog.conf /etc/nginx/sites-available
COPY nginx.pid /var/log/nginx
COPY nginx.conf /etc/nginx

# Create the symbolic link
RUN ln -s /etc/nginx/sites-available/blog.conf /etc/nginx/sites-enabled/blog.conf \
    # Claim ownership for the nginx user
    && chown -R nginx:nginx /usr/share/nginx /var/log/nginx /etc/nginx