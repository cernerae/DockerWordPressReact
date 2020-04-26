# --- Build React App ---
FROM node:11.12.0 as builder

# Name of React app folder
ARG react_dir

# Create working directory
RUN mkdir /usr/src/app
WORKDIR /usr/src/app

# Create constant environment variables
ENV PATH /usr/src/app/node_modules/.bin:$PATH

COPY $react_dir/package.json .
# COPY $react_dir/package-lock.json .

COPY $react_dir/. /usr/src/app

# Install
RUN npm install
# RUN npm --prefix ./$react_dir install react-scripts -g ./$react_dir

# Build app
RUN npm run build --silent

# --- Build NGINX ---
FROM nginx:1.15.2-alpine

# Nginx config
#RUN rm -rf /etc/nginx/conf.d
RUN rm -rf /etc/nginx/nginx.conf
RUN rm -rf /etc/nginx/conf.d
RUN mkdir /etc/nginx/conf.d
COPY nginx.conf /etc/nginx/nginx.conf
COPY default.conf /etc/nginx/conf.d/default.conf

# Static build
COPY --from=builder /usr/src/app/build /usr/share/nginx/html/

# Default port exposure
EXPOSE 80

# -- Install bash
RUN apk add --no-cache bash

# Start Nginx server
CMD ["nginx", "-g", "daemon off;"]