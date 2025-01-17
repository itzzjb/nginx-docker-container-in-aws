# We will start from a node.js base image
# We are using this image as a builder image to build the application
FROM node:lts as builder

# Create a directory to hold the application code inside the image
WORKDIR /usr/src/app

# By setting NODE_ENV to production, it will install only production dependencies. Dev dependencies will not be installed.
ENV NODE_ENV=production

# Copy package.json and package-lock.json to the image
COPY ./package*.json /usr/src/app
 
# By adding --production flag, it will install only production dependencies.

# We can use npm ci instead of npm install to install the dependencies faster.
# When using ci, the node_modules directory will be deleted and recreated acording to the package-lock.json file.
# If we don't have the package-lock.json file of if the package-lock.json and package.json files are out of sync, it will fail.
# Package-lock.json file is created when we run npm install.
# And when using package-lock.json file instead of package.json file, we can be sure that the dependencies are the same as the last time we ran npm install.
# When using npm install, the node_modules directory will be updated (added and removed on changes done) according to the package.json file.
# The word ci stands for clean-install.

# We use USER 0 to run the npm ci command as root user.
USER 0
RUN npm ci --production

# Copy the rest of the application code to the image
# Created a USER node to run the application as a non-root user called node.
USER node
COPY ./ /usr/src/app

# Now we are going to create a new image from the node:lts-alpine base image which is much smaller, less vulnerable than the node:lts image.
FROM node:lts-alpine

# Create a directory to hold the application code inside the image
WORKDIR /usr/src/app

# Copy the code from the builder image to the new image
COPY --from=builder /usr/src/app /usr/src/app

# Expose the port that the app runs on
EXPOSE 3000

# Start the application
CMD ["npm","start"] 