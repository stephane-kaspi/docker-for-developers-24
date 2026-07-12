# Production image for the course app.
# Single stage on purpose: this app has no build/compile step to isolate.
FROM node:20-alpine

WORKDIR /app

# Dependencies first — this layer is cached until package*.json changes
COPY package*.json ./
RUN npm ci --omit=dev

# Then the source — editing code won't bust the dependency layer above
COPY --chown=node:node . .

ENV NODE_ENV=production
EXPOSE 3000

# Drop root before the app runs
USER node

# Let Docker/Compose know when the app is actually ready (not just started)
HEALTHCHECK --interval=10s --timeout=3s --start-period=5s \
  CMD node -e "require('http').get('http://localhost:3000/health',r=>process.exit(r.statusCode===200?0:1)).on('error',()=>process.exit(1))"

CMD ["node", "server.js"]