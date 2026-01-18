# Use the official actual-server image as base
ARG BUILD_FROM=actualbudget/actual-server:25.7.0
FROM ${BUILD_FROM}

# Install nginx with lua module
USER root
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        nginx \
        libnginx-mod-http-lua \
        lua-cjson \
        supervisor && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Copy nginx config
COPY rootfs/etc/nginx/nginx.conf /etc/nginx/nginx.conf

# Copy supervisor config
COPY rootfs/etc/supervisor/supervisord.conf /etc/supervisor/supervisord.conf

# Create necessary directories
RUN mkdir -p /var/log/supervisor /var/log/nginx /run/nginx

# Set environment for actual-server to use internal port
ENV ACTUAL_PORT=5007
ENV ACTUAL_DATA_DIR=/data

# Expose ingress port (nginx listens here)
EXPOSE 5006

# Use supervisor to run both services
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]
