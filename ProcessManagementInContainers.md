# Process management in containers

You might have used nodemon, forever, or pm2, which manage node proecss,
make sure it's running.

No need for these in the container!

Docker can do start, stop pause, restart, and even health checks!

Node multi-threading, node is single thread, above tools did that, Docker
has this, called "replica"s

# Shutdown

Out of box, node and npm don't respond to "proper" termination signals

# PID 1

Process identifier 1 is the first process in a system (or container). Aka "init" ?
In Linux that means it's special. Init process has two jobs:
- It needs to handle "zombie proceses" - processes without parents, subprocess
  launched by parent, but parent has crashed. PID 1 is supposed to reap those
  processes
- Init process passes signals to subprocesses

If you research and find "you shouldn't node in a container" that's flat out wrong

Zombie processes aren't really an issue with node. Never seen this issue.
Shutdown is the second issue...

Shutdown commands to container come in 3 flavors (SIGINT, SIGTERM, SIGKILL)
 - Don't want to deal with SIGKILL - container doesn't even see this signal,
   it's just terminated
 - INT and TERM are passed into container. SIGINT = ctrl-c, SIGTERM = docker container stop, rolling updates
 - App has a chance to properly shut down

With node, we want ot make sure it cleans up files it may be writing, and finish
any connections that are open

npm doesn't handle these signals at all! Doesn't do anything gracefully

Node doesn't handle these signals by default! But you can add signal handling

"tini" - Docker has a "init PID 1 replacement option". Docker has a built in
init process. It's a backup or workaround.

# Proper shutdown options

If you see hitting ctrl-c and it waits about 10 seconds to shut down, that's
docker waiting 10 seconds, seeing the app doesn't handle sigint, and then it's
forced to send a sigkill!!

Options:
1. Use --init "shim", lets docker handle sigterm/sigint and will stop
   your node process (i 'm not sure what happens though). Might do this as last
   resort if you don't want to handle someone else's docker image custom code
    - docker run --init -d nodeapp <<<< shims in tini that wraps node app
      This is not properly handling node connections
2. Add tini to image in Dockerfile. Workaround. This will terminate everyone's
   connections. If not able to do long term code solution, and can't change
   app yourself, maybe your the ops person
    - ENTRYPOINT ["/sbin/tini", "--"]
    - using ENTRYPOINT instead of changing CMD, is nice trick because entrypoint
      wraps CMD and doesn't need to change CMD
3. Handle in app. See [sample-graceful-shutdown/sample.js](sample-graceful-shutdown/sample.js)
   Specific to each app. Things to handle: Counting connections and connection rollover
