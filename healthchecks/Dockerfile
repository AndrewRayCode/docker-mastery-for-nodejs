FROM node:10-alpine

EXPOSE 3000

RUN apk add --no-cache tini curl

# These are options, they overwrite each other, you can pick one
# Healthchecks run in container themselves

# option 1, using curl to GET the default app url
# NOTE: ensure curl is installed in image
# With pipes at end, forces curl to return a success, or forces error code
HEALTHCHECK --interval=5m --timeout=3s \
  CMD curl -f http://localhost/ || exit 1

# option 2, using curl to GET a custom url with app logic
# NOTE: ensure curl is installed in image
HEALTHCHECK CMD curl -f http://localhost/healthz || exit 1

# option 3, a custom code healthcheck that could
# do a lot more things then a simple curl
# or simply avoid needing curl to begin with
# Runs a custom command! still runs in the container. can do a lot more complex
# stuff if you want
# Have to think about third party tools to see what they're going to check too
# Rule of thumb: always have SOME health check
# If there's no http endpoint to hit, then how about a custom javascript health
# check that drops a file in a container and the hc script below checks for that
# file and ensures it's working as expected
HEALTHCHECK --interval=30s CMD node hc.js

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install && npm cache clean --force

COPY . .

ENTRYPOINT ["tini", "--"]

CMD ["node", "app.js"]
