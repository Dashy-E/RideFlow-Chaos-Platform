# ---- builder ----
FROM node:20-alpine AS builder
WORKDIR /build
COPY package.json ./
RUN npm install --omit=dev --no-audit --no-fund

# ---- runtime ----
FROM node:20-alpine
RUN addgroup -S app && adduser -S -G app -u 10001 app
WORKDIR /app
COPY --from=builder /build/node_modules ./node_modules
COPY --chown=app:app package.json ./
COPY --chown=app:app src/ ./src/
USER 10001
EXPOSE 8081
HEALTHCHECK --interval=30s --timeout=3s CMD node -e "require('http').get('http://localhost:8081/healthz',r=>process.exit(r.statusCode===200?0:1)).on('error',()=>process.exit(1))"
CMD ["node", "src/index.js"]
