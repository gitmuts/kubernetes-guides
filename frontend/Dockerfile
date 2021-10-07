#build command docker build --build-arg env=sit -t gidimuts/adminfrontend:sit .
# build stage
FROM node:lts-alpine as build-stage
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
COPY default.conf ./

ARG env=dev
ARG devurl=http://YOUR_DEV_IP/api/
ARG siturl=http://YOUR_SIT_IP/api/

RUN if [ "$env" = "dev" ] ; then export VUE_APP_BASE_URI=${devurl} && npm run build ;fi
RUN if [ "$env" = "sit" ] ; then export VUE_APP_BASE_URI=${siturl} && npm run build ;fi

# production stage
FROM nginx:stable-alpine as production-stage
COPY --from=build-stage /app/default.conf /etc/nginx/conf.d/default.conf
COPY --from=build-stage /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
