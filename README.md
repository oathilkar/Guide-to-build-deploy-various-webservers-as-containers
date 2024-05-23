# Guide-to-build-deploy-various-webservers-as-containers

Building and deploying various web servers as containers is a great way to ensure consistency and ease of deployment across different environments. 
Here's a detailed guide on how to build and deploy three popular web servers as Docker containers: Nginx, Apache HTTP Server, and Node.js using Express.

### 1. Nginx Server

#### Dockerfile for Nginx

Create a `Dockerfile` in your project directory:

```dockerfile
# Use an existing Docker image as a base
FROM nginx:latest

# Copy local files to the container's filesystem
COPY ./nginx.conf /etc/nginx/nginx.conf
COPY ./index.html /usr/share/nginx/html/index.html

# Expose a port to allow incoming traffic
EXPOSE 80

# Command to run your application
CMD ["nginx", "-g", "daemon off;"]
```

#### nginx.conf

Create an `nginx.conf` file in the same directory as your `Dockerfile` if you have custom configurations:

```nginx
# Example nginx.conf
server {
    listen 80;
    server_name localhost;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }
}
```

#### Build and Run Nginx Container

Open a terminal and execute the following commands:

```bash
# Build Docker image
docker build -t my-nginx .

# Run Docker container
docker run -d -p 80:80 my-nginx
```

### 2. Apache HTTP Server

#### Dockerfile for Apache

Create a `Dockerfile` for Apache:

```dockerfile
# Use an existing Docker image as a base
FROM httpd:latest

# Copy local files to the container's filesystem
COPY ./index.html /usr/local/apache2/htdocs/index.html
COPY ./httpd.conf /usr/local/apache2/conf/httpd.conf

# Expose a port to allow incoming traffic
EXPOSE 80

# Command to run your application
CMD ["httpd-foreground"]
```

#### httpd.conf

Create an `httpd.conf` file in the same directory as your `Dockerfile` for any custom configurations:

```apacheconf
# Example httpd.conf
ServerName localhost
```

#### Build and Run Apache Container

Open a terminal and execute the following commands:

```bash
# Build Docker image
docker build -t my-apache .

# Run Docker container
docker run -d -p 80:80 my-apache
```

### 3. Node.js with Express

#### Dockerfile for Node.js

Create a `Dockerfile` for Node.js:

```dockerfile
# Use an existing Docker image as a base
FROM node:latest

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
COPY package*.json ./
RUN npm install

# Bundle app source
COPY . .

# Expose a port to allow incoming traffic
EXPOSE 3000

# Command to run your application
CMD ["node", "app.js"]
```

#### app.js (Express Application)

Create an `app.js` file for your Express application:

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello World!');
});

const port = process.env.PORT || 3000;
app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```

#### package.json

Create a `package.json` file to define your Node.js application:

```json
{
  "name": "my-express-app",
  "version": "1.0.0",
  "description": "A simple Express.js app",
  "main": "app.js",
  "dependencies": {
    "express": "^4.17.2"
  }
}
```

#### Build and Run Node.js Container

Open a terminal and execute the following commands:

```bash
# Build Docker image
docker build -t my-node-app .

# Run Docker container
docker run -d -p 3000:3000 my-node-app
```

### Summary

- **Nginx**: Ideal for serving static content and as a reverse proxy.
- **Apache**: Flexible and widely used, suitable for dynamic content and PHP applications.
- **Node.js with Express**: Great for building APIs and web applications.

Adjust these examples to fit your specific needs, including custom configurations, additional services, or optimizations. Always consider security and best practices when exposing containers to the internet.
