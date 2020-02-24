# Startup order and dependencies

docker-compose "depends_on" feature has changed between docker-compose 2 and 3.
We get more out of the box with docker-compose v2.

depends_on solves:
 - DNS
   - If your app starts up and searches for a database name, and you get
     the error "can't resolve service_name", that's what depends_on fixes.
     depends_on means they're started first, and that means DNS will start
     first and resolve to IPs on the docker network
   - This is only for compose, not orchestrating!
   - v2 has health checks, v3 doesn't have this!
 - Conncetion Failure Handling
   - can use restart: on-failure in dockerfile, but not as good as depends_on(?)
   - restart is docker restarting the container if it gets a bad exit code / crashes
   - "restart" could get into restart loop and constantly restart, and get CPU spikes

Solution:
 - build connection timeouts, buffers, and retries in apps
 - Most db connectors (in node) can both buffer connections and wait for back end
   to come back up, and can retry in case of failures
 - Building this into app will make your app more resiliant and not have worse
   problems than you do in local development

Reminder depends_on health checks are v2

Online people will talk about scripts
