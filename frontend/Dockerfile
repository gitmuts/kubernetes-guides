# build stage....This is a multi stage build
#In this stage we instruct docker to download node js with the OS alphine installed
FROM node:lts-alpine as build-stage  
#Create a directory /app
WORKDIR /app

#We copy our requirements file, this file normally contains dependencies/ external libaries our application uses....for JAVA pom.xm
COPY package*.json ./
#The next command installs the depencies defined above
RUN npm install
#The command COPY . ., copies all files in our current directory to the container's directory
COPY . .
#Copy nginx configuration 
COPY default.conf ./

#We now build our application
RUN  npm run build

# production stage
#Download nginx image from docker hub, we want the version that has alphine OS
FROM nginx:stable-alpine as production-stage
#Copy the default.conf copied in step 13 to nginx conf directory
COPY --from=build-stage /app/default.conf /etc/nginx/conf.d/default.conf
#Copy static files generated after running step 17, to a directory nginx will be able to read as defined in default.conf file
COPY --from=build-stage /app/dist /usr/share/nginx/html
#Instruct docker that we will be exposing port 80
EXPOSE 80
#Start command for nginx......JAVA will be something like java -jar JAR_FILE.jar
CMD ["nginx", "-g", "daemon off;"]
#build command docker build -t gidimuts/frontend:1.0 .
