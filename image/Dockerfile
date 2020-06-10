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

# Copy several files to /app
COPY ednon-hackaboss/package*.json ./
COPY ednon-hackaboss/yarn* ./
COPY ednon-hackaboss/babel.config.js ./

# We install Yarn
RUN yarn install

# Copy our application 
COPY ednon-hackaboss /app

# Build
RUN yarn run build

# We use NGINX 
FROM nginx:stable-alpine

# Copy our application from build-stage
COPY --from=build-stage /app/dist /usr/share/nginx/html

# Run NGINX
CMD ["nginx", "-g", "daemon off;"]