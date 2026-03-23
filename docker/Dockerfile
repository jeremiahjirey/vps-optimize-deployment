# ----------- Stage 1: Dependencies -----------
FROM node:20-alpine AS deps

WORKDIR /app

# Copy only package files (better caching)
COPY backend/package*.json ./

# Install only production dependencies
RUN npm ci --omit=dev

# ----------- Stage 2: Runner -----------
FROM node:20-alpine

WORKDIR /app

# Create non-root user (security best practice)
RUN addgroup -S nodejs && adduser -S appuser -G nodejs

# Copy node_modules from deps stage
COPY --from=deps /app/node_modules ./node_modules

# Copy app source
COPY backend/ .

# Set ownership
RUN chown -R appuser:nodejs /app

USER appuser

# Expose port
EXPOSE 3000

# Start app
CMD ["node", "server.js"]