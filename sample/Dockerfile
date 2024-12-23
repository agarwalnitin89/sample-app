# Stage 1: Build stage
FROM node:18 as builder

# Set working directory
WORKDIR /app

# Install system packages (simulates build-time dependencies)
RUN apt-get update && apt-get install -y \
    build-essential \
    python3 \
    git \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Copy package.json and package-lock.json
COPY package*.json ./
# COPY ../sample ./
# Install dependencies
RUN npm install
RUN ls -la
# Copy the application code
COPY . .

RUN ls -la

# Build the application
RUN npm run build

# Stage 2: Production stage
FROM node:18-slim

# Set working directory
WORKDIR /app

# Install minimal system packages
RUN apt-get update && apt-get install -y \
    curl \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Copy necessary files from the builder stage
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist

# Expose the application port
EXPOSE 3000

# Set environment variables
ENV NODE_ENV=production

# Command to run the application
CMD ["node", "dist/index.js"]

