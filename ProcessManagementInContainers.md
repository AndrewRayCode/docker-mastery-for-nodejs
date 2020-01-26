# Process management in containers

You might have used nodemon, forever, or pm2, which manage node proecss,
make sure it's running.

No need for these in the container!

Docker can do start, stop pause, restart, and even health checks!

Node multi-threading, node is single thread, above tools did that, Docker
has this, called "replica"s

# Shutdown

Out of box, node and npm don't respond to "proper" termination signals