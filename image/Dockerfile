# build stage
FROM node:12.6.0-alpine as build-stage

LABEL maintainer="manuel.suarez@ednon.es"
LABEL organization="ednonlabs.com"

WORKDIR /app
COPY ednon-hackaboss/package*.json ./
COPY ednon-hackaboss/yarn* ./
COPY ednon-hackaboss/babel.config.js ./

ARG GIT_TAG 
ENV VERSION=$GIT_TAG

RUN yarn install
COPY ednon-hackaboss /app
RUN yarn run build

# production stage
FROM nginx:stable-alpine
COPY --from=build-stage /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]