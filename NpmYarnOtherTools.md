# NPM, Yarn, other tools

# docker-compose run

Starts a new container and runs command

# docker-compose exec

Run command on existing container
- Used for one-off commands, and shells in existing container

docker-compose exec and run assume -it

He just does docker-compose exec to run commands inside the container. The
strapi api service in this folder breaks and he fixed it two days ago, i don't care enough to merge his master into mine