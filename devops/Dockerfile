# Use the latest version of the Node.js image as the base image
FROM node:latest  

# Set the working directory inside the container
WORKDIR /app

# Install npm package manager
RUN apt-get update && apt-get install -y npm

# Copy package.json and package-lock.json files to the container
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code to the container
COPY . .

# Expose the port the app runs on
EXPOSE 3000

# Command to run the application
CMD ["npm", "start"]

