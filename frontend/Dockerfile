# Use official lightweight Nginx image
FROM nginx:alpine

# Copy our HTML into Nginx’s default html folder
COPY index.html /usr/share/nginx/html/index.html

# Expose port 80 so we can map it on the host
EXPOSE 80

# Start Nginx in the foreground
CMD ["nginx", "-g", "daemon off;"]
