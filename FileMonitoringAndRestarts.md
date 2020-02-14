# Nodemon

Can create a nodemon.yml for each tool, and can find examples online for each tool

- Not installing nodemon on the host, installing in
container as devDependency
- COULD do npm install nodemon global in the Dockerfile, but that sucks, prefer to install in devDependencies

# First

Installed nodemon in container

docker-compose run express npm install nodemon --save-dev