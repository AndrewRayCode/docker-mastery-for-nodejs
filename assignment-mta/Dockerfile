FROM node:8.17.0-alpine3.11

RUN apk update && apk add --no-cache tini graphicsmagick

WORKDIR /node

RUN mkdir app && chown -R node:node app

WORKDIR /node/app

COPY package*.json ./

RUN npm install && npm cache clean --force

COPY . .

USER node

ENTRYPOINT [ "/sbin/tini", "--" ]

# Putting near bottom so rebuilding will make less build
ENV CHARCOAL_FACTOR 1

CMD [ "node", "index.js" ]
