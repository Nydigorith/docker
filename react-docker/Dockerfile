# create react file first


# set the base image to create the image for react app
FROM node:20-alpine

# create a user with permissions to run the app
# -S -> create a system user
# -G -> add the user to a group
# This is done to avoid running the app as root
# If the app is run as root, any vulnerability in the app can be exploited to gain access to the host system
# It's a good practice to run the app as a non-root user

# RUN addgroup app (group name) && adduser -S -G app (will be added to app group) app(username)
RUN addgroup app && adduser -S -G app app

# set the user to run the app
# switch to the non-root user
USER app

# set the working directory to /app
WORKDIR /app

# copy package.json and package-lock.json to the working directory
# This is done before copying the rest of the files to take advantage of Docker’s cache
# If the package.json and package-lock.json files haven’t changed, Docker will use the cached dependencies
# COPY package*.json ./ (same with .) 
COPY package*.json ./ 

# sometimes the ownership of the files in the working directory is changed to root
# and thus the app can't access the files and throws an error -> EACCES: permission denied
# to avoid this, change the ownership of the files to the root user
USER root

# change the ownership of the /app directory to the app user
# chown -R <user>:<group> <directory>
# chown command changes the user and/or group ownership of for given file.
RUN chown -R app:app .

# change the user back to the app user
USER app

# install dependencies
RUN npm install

# copy the rest of the files to the working directory
COPY . .

# expose port 5173 to tell Docker that the container listens on the specified network ports at runtime
EXPOSE 5173

# command to run the app
CMD npm run dev

# build the image
# docker build -t react-docker .
    # -t -> tag the image with a name
    # react-docker -> name of the image
    # . -> path to the Dockerfile, current directory on the host machine

# run the image using port mapping, (will not work if package json  vite --host is not set, rebuild image since pakceage json is changed)
# docker run -p 5173:5173 react-docker (rerunning if the container is running will cause error)
    # -p -> port mapping
    # 5173:5173 -> map port 5173 on the host machine to port 5173 on the container
    # react-docker -> name of the image

# docker container prune -> remove all stopped containers

#  run the image and automatically refresh the app when the code changes
# docker run -p 5173:5173 -v "$(pwd):/app" -v /app/node_modules react-docker  
    # -v -> creating a volume, volume mapping, keep the files in sync between the host machine and the container
    # $(pwd) -> current directory on the host machine
    # /app -> current directory inside the container
    # react-docker -> name of the image
    # /app/node_modules -> volume to store the node_modules directory, this is done to avoid installing the dependencies every time the container is run




# publish the image to docker hub
# docker login
    # automatically authenticated if logged in to docker desktop

# tag the image
# docker tag react-docker <docker-hub-username>/react-docker
    # react-docker -> name of the image
    # <docker-hub-username> -> username of the docker hub account

# push the image to docker hub
# docker push <docker-hub-username>/react-docker
    # <docker-hub-username> -> username of the docker hub account
    # react-docker -> name of the image

