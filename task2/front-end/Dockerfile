# Use the official Nginx image
FROM nginx:latest

# Copy static front-end files into the Nginx web root
COPY ./softy-pinko-front-end /var/www/html/softy-pinko-front-end

# Replace the default Nginx configuration
COPY ./softy-pinko-front-end.conf /etc/nginx/conf.d/default.conf
