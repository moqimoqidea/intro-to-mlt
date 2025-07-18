# Build stage
FROM node:18-alpine AS build

# Accept build arguments
ARG REACT_APP_API_URL
ARG REACT_APP_ALLOY_ENDPOINT

# Set as environment variables for the build process
ENV REACT_APP_API_URL=$REACT_APP_API_URL
ENV REACT_APP_ALLOY_ENDPOINT=$REACT_APP_ALLOY_ENDPOINT

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install all dependencies (including dev dependencies needed for build)
RUN npm ci

# Copy source code
COPY . .

# Build the React app with rsbuild
RUN npm run build

# Production stage
FROM nginx:alpine

# Copy custom nginx config
COPY nginx.conf /etc/nginx/nginx.conf

# Copy built app from build stage
COPY --from=build /app/build /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start nginx
CMD ["nginx", "-g", "daemon off;"]
