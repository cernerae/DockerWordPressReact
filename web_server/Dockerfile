# --- Build React App ---
FROM node:13.13.0 as builder
# Name of React app folder
ARG react_dir

# Create working directory
RUN mkdir /usr/src/app
WORKDIR /usr/src/app

# Create constant environment variables
ENV PATH /usr/src/app/node_modules/.bin:$PATH
# Copy our React files
COPY $react_dir/. /usr/src/app

# Install
RUN npm install

# Build app
RUN npm run build --silent

# --- Build NGINX ---
FROM nginx:1.17.10-alpine
# Nginx config
RUN rm -rf /etc/nginx/nginx.conf
RUN rm -rf /etc/nginx/conf.d
RUN mkdir /etc/nginx/conf.d
COPY nginx.conf /etc/nginx/nginx.conf
COPY default.conf /etc/nginx/conf.d/default.conf

# Static build
COPY --from=builder /usr/src/app/build /usr/share/nginx/html/

# Default port exposure
EXPOSE 80

# Start Nginx server
CMD ["nginx", "-g", "daemon off;"]