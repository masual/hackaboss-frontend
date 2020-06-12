# We specify the base image we need for our frontend application
FROM node:12.6.0-alpine as build-stage

# We specify Dockerfile's maintener
LABEL maintainer="manuel.suarez@ednon.es" \
      organization="ednonlabs.com"

# Set GIT_TAG in order to show application's version
ARG GIT_TAG 
ENV VERSION=$GIT_TAG

# Information about app port
EXPOSE 80

# Set the current working directory inside the container
WORKDIR /app

# We copy the dependency specification and install them first 
# in order to speed up the building of the image when changes
# are made to the actual code, that is copied on the next step
COPY ednon-hackaboss/package*.json ./
COPY ednon-hackaboss/yarn* ./
COPY ednon-hackaboss/babel.config.js ./
RUN yarn install

# Copy our application 
COPY ednon-hackaboss /app

# Build for production with Yarn
RUN yarn run build

# We use NGINX to serve the application
FROM nginx:stable-alpine

# We add a custom configuration
COPY image/nginx-config/default.conf /etc/nginx/conf.d/default.conf

# Copy our compiled application from build-stage
COPY --from=build-stage /app/dist /usr/share/nginx/html

# Run NGINX
CMD ["nginx", "-g", "daemon off;"]