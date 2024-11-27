# Base image for the frontend
FROM node:22-slim AS base

# Set working directory
WORKDIR /frontend

# Install debugging tools
RUN apt update && apt install -y bash net-tools

# Copy package.json and package-lock.json separately for better caching
COPY package*.json ./

# Install dependencies and clear cache
RUN npm install && npm cache clean --force

# Base image for stage 2 to reduce size of image
FROM node:22-slim

# Set working directory
WORKDIR /frontend

# Copy dependencies and source code
COPY --from=base /frontend/node_modules ./node_modules

# Expose application port
EXPOSE 3000

# Copy the rest of the application files
COPY . .

# Command to start the application
CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0", "--port", "3000"]