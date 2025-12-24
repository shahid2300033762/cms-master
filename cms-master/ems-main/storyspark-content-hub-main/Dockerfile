# Step 1: Build Vite React frontend
FROM node:18 AS build
WORKDIR /app

# Accept build arguments
ARG VITE_API_URL
ARG VITE_BASE_PATH

# Set environment variables
ENV VITE_API_URL=${VITE_API_URL}
ENV VITE_BASE_PATH=${VITE_BASE_PATH}

# Copy package files first for better caching
COPY package*.json ./

# Install dependencies with legacy peer deps to avoid conflicts
RUN npm install --legacy-peer-deps

# Copy source code
COPY . .

# Build the application
RUN npm run build

# Step 2: Serve using Nginx (production)
FROM nginx:alpine
RUN apk add --no-cache wget
COPY --from=build /app/dist /usr/share/nginx/html

# Copy custom nginx configuration
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
