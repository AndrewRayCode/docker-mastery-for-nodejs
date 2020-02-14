# 12 factor
- https://12factor.net - 12 factor app <- go read it!
- Heroku came up with this pattern, was around before docker
- docker used to be "dot cloud" and came up with docker and gave it away for free
- containers are almost always distributed apps
- Docker gives you a lot of 12 factor for free but not all of them

# Config
- store all config in env vars so it will pick it up if set
- All env variables should be defined in dockerfile (Lol)
- the command envsubst can make config files out of env vars for legacy apps (I think?)
- Anything that would change in a different environment is a config

# Logging
- Don't make fancy part of app to transfer logs somewhere
- Don't write logs to logfiles, don't use third party tool, just use STDOUT/STDERR
- Winston is default, but console.log works
- Winston can "scale" logging, by letting you configure log levels
- If using winston, only use "console" transport

# what ur container needs
 - use env vars for config
 - log to stdout/err
 - pin versions, including npm
 - graceful exit with sigterm/sigint

# Dockerignore
.git
node_modules
npm-debug
docker-compose*.yml <- can leak more information than desired, like env vars and app setup

He likes README in container
Dockerfile too for checking out how app was built - can easily compare two images

docker ps -l (lower case L) is last container that ran