# frontend/Dockerfile
FROM node:20-alpine as build

# Set the working directory
WORKDIR /app

# Copy the package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the application code
COPY . .

# Use environment variables during build
# ARG VITE_API_BASE_URL
# ENV VITE_API_BASE_URL=${VITE_API_BASE_URL}


# Build the application
RUN npm run build

# Production image
FROM nginx:stable-alpine

# Remove default NGINX configuration
RUN rm -rf /etc/nginx/conf.d

# Copy custom NGINX configuration
COPY nginx.conf /etc/nginx/nginx.conf

# Copy the build output to the Nginx HTML folder
COPY --from=build /app/dist /usr/share/nginx/html

# Expose the port the app runs on
EXPOSE 80

# Command to run Nginx
CMD ["nginx", "-g", "daemon off;"]
