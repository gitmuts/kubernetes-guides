# build stage
FROM node:lts-alpine as build-stage
WORKDIR /app

COPY package*.json ./
RUN npm install
COPY . .
COPY default.conf ./

RUN  npm run build

# production stage
FROM nginx:stable-alpine as production-stage
COPY --from=build-stage /app/default.conf /etc/nginx/conf.d/default.conf
COPY --from=build-stage /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
#build command docker build -t gidimuts/frontend:1.0 .