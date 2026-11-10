# Stage 1: Build
FROM node:18-alpine AS builder

WORKDIR /app

# Copy dependency files
COPY package*.json ./

# Install dependencies
RUN npm ci

# Copy application source
COPY . .

# Receive API URL at build time
ARG NEXT_PUBLIC_API_URL
ENV NEXT_PUBLIC_API_URL=$NEXT_PUBLIC_API_URL

# Build Next.js application
RUN npm run build


# Stage 2: Runtime
FROM node:18-alpine AS runtime

WORKDIR /app

# Copy application
COPY --from=builder /app /app

EXPOSE 3000

# Start Next.js production server
CMD ["npm", "run", "start"]