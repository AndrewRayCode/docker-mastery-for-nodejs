# all official core images for node on dockerhub stem from DEBIAN get apt-get
# package manager out of the box

# Start with debian if migrating, starting with alpine if new

# Don't use :latest
# Stick with even numbered releases, odd numbered releases are experimental
# node:10 is debian. 10.x < x is patch version
# Don't use ":slim" <- alpine is for small builds, not slim
# Don't use ":onbuild" <- a dockerfile command we don't use

# Start with a generic version like 10 for development, move to a patch version
# as you get closer to production
FROM node:10-alpine

# Run this command high up, as this will rarely change. And pin package versions!
RUN apk add --update curl
# If you do an apk add LATER in the file, it will be technically installing
# something from the "old version of the apt-get update" (??), beacuse the above
# line wouldn't have been cache busted

# This line is never going to change, so put it and others like it near the top
EXPOSE 3000

# Don't use CD and MKDIR, workdir which makes and cds to directory
# if you need specific perms on a directory, mkdir might be better
WORKDIR /usr/src/app

# Can put an asterisk at the end of the filename in case it doesn't exist!
COPY package.json package-lock.json* ./ 

# npm is built into base image (so is yarn?), no need to upgrade or install

# clean up after yourself, clean --force cleans everything related to
# cache to make everything is small as possible
RUN npm install && npm cache clean --force

# use COPY, not ADD. They do the same thing essentially, but ADD can do a lot
# more stuff, but can do a lot more and gets confusing
COPY . .

# Note if order of above lines were reversed, any time you changed a source file
# the layer below would have to re-run, so it would run npm install after every
# file change. That's why the copy of package.json and package-lock.json
# happen before the npm install too, only if those change

# use node, not npm!
# if you run with npm, you need another process to run (npm)
# npm isn't as literal, dockerfiles should be literal and specific
# npm doesn't work well as an "init process" or "PID 1" process
# npm doesn't handle sigterm correctly, doesn't properly pass shutdown commands to node!

# Docker images run as root in container by default. One way to avoid that is to
# run node as non-root user, so even if someone got into container, they
# wouldn't have privileges. Node images have a node user but it's not on by
# default

# Do it AFTER package installs and npm install -g, but BEFORE npm install
# May require "chown node:node" if your app needs directory writes

# This is all you have to do! This only applies to RUN, CMD, ENTRYPOINT, but not WORKDIR
# so set permissions on app dir:
RUN mkdir app && chown -R node:node .
# You can exec as root with "docker-compose exec -u root" if the above fails

# Official images have node user built in!
# Not enabled by default
# Advice: Enable least-privilege node user by default
USER node

# Only RUN, CMD, and ENTRYPOINT actually use node user. Other commands, like WORKDIR, don't
CMD [ "node", "./bin/www" ]
