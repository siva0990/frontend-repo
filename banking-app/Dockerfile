# ── Stage 1: Build ───────────────────────────────────────────────
FROM node:20-alpine AS build
WORKDIR /app

# Copy lockfiles first for layer caching
COPY package.json package-lock.json ./
RUN npm ci

# Copy source and build for production
COPY . .
RUN npx ng build --configuration production

# ── Stage 2: Serve ───────────────────────────────────────────────
FROM nginx:1.27-alpine AS runtime

# Remove default Nginx page
RUN rm -rf /usr/share/nginx/html/*

# Copy Angular build output
COPY --from=build /app/dist/banking-app/browser /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
