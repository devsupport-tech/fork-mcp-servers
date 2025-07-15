# ---- Builder Stage ----
FROM node:22.12-alpine AS builder

WORKDIR /app

# Copy everything from /src/filesystem
COPY . .

# Install all dependencies without lifecycle scripts
RUN npm install --ignore-scripts

# Compile TypeScript using local tsconfig.json
RUN npx tsc -p ./tsconfig.json

# ---- Release Stage ----
FROM node:22-alpine AS release

WORKDIR /app

# Only bring in compiled code and required packages
COPY --from=builder /app/dist /app/dist
COPY --from=builder /app/package.json /app/package.json
COPY --from=builder /app/package-lock.json /app/package-lock.json

ENV NODE_ENV=production

# Install only production dependencies
RUN npm ci --ignore-scripts --omit=dev

# Launch the server
ENTRYPOINT ["node", "/app/dist/index.js"]
