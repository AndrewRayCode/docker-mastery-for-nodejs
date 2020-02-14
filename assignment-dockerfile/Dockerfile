# fill in the blanks to dockerize this node app
FROM node:12.14.1-alpine

EXPOSE 3000

RUN apk update && apk add --no-cache tini

WORKDIR /node

# LOL make this standard across company
RUN mkdir app && chown -R node:node .

WORKDIR /node/app

COPY package.json package-lock.json* ./

USER node

RUN npm install && npm cache clean --force

COPY . .

# Can do this or init: true in dockerfile or run docker with --init to shim in
# tini
ENTRYPOINT ["/sbin/tini", "--"]

CMD ["node", "app.js"]
