# ---------- Build Stage ----------
FROM node:22.12-alpine AS builder

WORKDIR /src/filesystem

COPY . .

RUN npm install --ignore-scripts

# Compile TypeScript
RUN npx tsc -p ./tsconfig.json

# ---------- Release Stage ----------
FROM node:22-alpine AS release

WORKDIR /app

COPY --from=builder /src/filesystem/dist /app/dist
COPY --from=builder /src/filesystem/package.json /app/package.json
COPY --from=builder /src/filesystem/package-lock.json /app/package-lock.json

RUN npm ci --ignore-scripts --omit=dev

ENV NODE_ENV=production

ENTRYPOINT ["node", "/app/dist/index.js"]
