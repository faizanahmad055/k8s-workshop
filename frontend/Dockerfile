# Use lightweight Nginx image as base
FROM nginx:alpine

# Install envsubst for substituting environment variables
RUN apk --no-cache add gettext

# Set the working directory in the container
WORKDIR /usr/share/nginx/html

# Copy the HTML file to the container
COPY index.html .

#COPY nginx.conf /etc/nginx/conf.d/

# Copy the config template
# COPY config.json .

# Expose port 80 to the outside world
EXPOSE 80

# Start Nginx server
CMD ["nginx", "-g", "daemon off;"]

