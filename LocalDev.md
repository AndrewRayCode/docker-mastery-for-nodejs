**Do** use docker-compose for local dev

Version in docker-compose is 1,2,3, 2 gives network and volumes, 3 gives us
clustering and orchestration. 2 is not outdated, 3 is a fork in the road. 3 is
for something like swarm / kubernetes, high availability, rolling updates.
Verison 2 has some features we don't have in version 3.

Stick with version 2 if doing local development

# V2 only
- depends_on, which doesn't make sense in a multi cluster setup. you can't have
  one thing depend on another in distributed system, and needs retry mechanisms
- Can do devices(?) and hardware things (?)

# Examples
[good.yml](compose-tips/good.yml) and [bad.yml](compose-tips/bad.yml) are examples

# Bind mounts

# Paths
Paths in docker-compose should assume pwd, left side of bind mount paths should
always start with ./ (or ..) and be a relative path

Don't use host file paths!

Don't bind mount databases! Not same filesystem!

Don't need to rebuild every time on every up

Bind mounts cross the system boundary, and they are slow. Macs have to tell
a linux vm a file has changed, can add up to 100ms of delay!

Volume can end with :delegated which says if any reading/writing goes in the
container, the host can catch up later (!). Good for npm installs in container.
Woudln't use for a database (probably), but wouldn't bind mount databases anyway

One option is file syncing, like docker-bg-sync, that runs a file copying
utility in background. Disadvantage is delay of files syncing, but for large
projects works for a lot of people.
