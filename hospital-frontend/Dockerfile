# Multi-stage build: Node (build) -> Nginx (serve)

# 1) Builder: install deps and build static assets
FROM node:20-alpine AS builder
WORKDIR /app

# Install dependencies first (leverage Docker layer cache)
COPY package*.json ./
# Prefer npm ci when lockfile exists
RUN if [ -f package-lock.json ]; then npm ci; else npm install; fi

# Copy all sources and build
COPY . .
# Accept optional Vite build-time envs via ARGs
ARG VITE_API_BASE_URL
ENV VITE_API_BASE_URL=${VITE_API_BASE_URL}
RUN npm run build

# 2) Runtime: Nginx serves built assets
FROM nginx:alpine AS runtime

# Replace default site with SPA-aware config
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy built artifacts
COPY --from=builder /app/dist /usr/share/nginx/html

# Expose container port
EXPOSE 80

# Healthcheck using busybox wget
HEALTHCHECK --interval=10s --timeout=3s --retries=3 --start-period=10s \
  CMD wget -q -O - http://localhost/ >/dev/null 2>&1 || exit 1

# Run nginx in foreground
CMD ["nginx", "-g", "daemon off;"]