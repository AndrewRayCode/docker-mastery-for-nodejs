# Has no devDependencies, and runs node
FROM node:10-slim as prod

# note this was made popular by express, not a standard node thing per say
ENV NODE_ENV=production

WORKDIR /node

RUN mkdir app && chown -R node:node .

WORKDIR /node/app

COPY package.json package-lock.json* ./

RUN npm install --only=prod && npm cache clean --force

COPY . .

USER node

CMD ["node", "./bin/www"]

# Dev runs nodemon
FROM prod as dev
ENV NODE_ENV=development

# Suggestion: Don't cache clean here, because we care more about small build
# times than small images. this image won't be shared with anyone
RUN npm install --only=dev

CMD ["./node_modules/nodemon/bin/nodemon.js", "./bin/www", "--inspect=0.0.0.0:9229"]

# Assignment onstructionz from video: Runs npm test, CMD = test, can run npm test
# or can manually type in test command npm test is more standard for npm users,
# if package.json file chagnes it changes here too
FROM dev as test
ENV NODE_ENV=development

# This could also be done from running this container with a specific test command
# He hints we won't really use this in the future for local development, but
# for CI you would use this
CMD ["npm", "test"]

# Could also just
RUN npm test

# This way if the image fails to build, you know the tests failed
