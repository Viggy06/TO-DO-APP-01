#STEPS TO RUN
# BUILD IMAGE USING COMMAND: docker build -t backend-image .
#THEN CHECN THE IMAGE using > docker images
    #backend-image                  latest          c1a2568c5db4   10 seconds ago   1.37GB
#docker run -d -p 3500:3500 -e MONGO_CONN_STR="mongodb://host.docker.internal:27017/tasksdb" -e PORT=3500 --name backend-container backend-image

#THEN docker ps to see the list of running container
#THEN VISIT to localhost:3000

# FROM node:14
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
CMD ["node", "index.js"]

#npm ci package-lock.json file is used to install exact versions of dependencies. 
#It is faster than npm install and is recommended for production builds.

#STAGE 1: Build Image
# FROM node:20-alpine AS 
# WORKDIR /app
# COPY package*.json ./
# RUN npm ci
# COPY . .
# EXPOSE 3500
# CMD ["node", "index.js"]
