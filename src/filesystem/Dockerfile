FROM node:22.12-alpine AS builder

WORKDIR /app

# Copy all source files
COPY . /app

# Install deps without running prepare (which breaks)
RUN npm install --ignore-scripts

# ✅ Compile TypeScript manually (safe and clean)
RUN npx tsc

FROM node:22-alpine AS release

WORKDIR /app

COPY --from=builder /app/dist /app/dist
COPY --from=builder /app/package.json /app/package.json
COPY --from=builder /app/package-lock.json /app/package-lock.json

ENV NODE_ENV=production

RUN npm ci --ignore-scripts --omit-dev

ENTRYPOINT ["node", "/app/dist/index.js"]
