FROM node:10-slim


EXPOSE 3000

# This creates as root, not user!
WORKDIR /node

COPY package*.json ./

# This is done as root..
RUN mkdir app && chown -R node:node .

# Then switch..
USER node

# Then install, as node user, should work
RUN npm install && npm cache clean --force

WORKDIR /node/app

COPY --chown=node:node . .


CMD [ "node", "app.js" ]
