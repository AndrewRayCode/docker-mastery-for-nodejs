## Stage 1 (production base)
# This gets our prod dependencies installed and out of the way
FROM node:10-alpine as base

EXPOSE 3000

ENV NODE_ENV=production

WORKDIR /opt

COPY package*.json ./

# we use npm ci here so only the package-lock.json file is used
RUN npm config list \
    && npm ci \
    && npm cache clean --force


## Stage 2 (development)
# we don't COPY in this stage because for dev you'll bind-mount anyway
# this saves time when building locally for dev via docker-compose
FROM base as dev

ENV NODE_ENV=development

ENV PATH=/opt/node_modules/.bin:$PATH

WORKDIR /opt

RUN npm install --only=development

WORKDIR /opt/app

CMD ["nodemon", "./bin/www", "--inspect=0.0.0.0:9229"]


## Stage 3 (copy in source)
# This gets our source code into builder for use in next two stages
# It gets its own stage so we don't have to copy twice
# this stage starts from the first one and skips the last two

# Intermediate stage, not used by us our ci/cd directly
FROM base as source

WORKDIR /opt/app

COPY . .


## Stage 4 (testing)
# use this in automated CI
# it has prod and dev npm dependencies
# In 18.09 or older builder, this will always run
# In BuildKit, this will be skipped by default 
FROM source as test

# Have to re-create because we're not depending on dev stage
ENV NODE_ENV=development
ENV PATH=/opt/node_modules/.bin:$PATH

# this copies all dependencies (prod+dev)
# copies in both dev and prod dependencnies
# He says to put unit tests in here, not functional tests, because those
# need to know where other things live, and it's hard to tell docker about those,
# this dockerfile is meant to be in isolation, not know about databases, etc
COPY --from=dev /opt/node_modules /opt/node_modules

# run linters as part of build
# be sure they are installed with devDependencies
RUN eslint . 

# run unit tests as part of build. Ah. This will block this build from building
# until the unit tests for this dockerfile have passed
RUN npm test

# If you want to run this image it will run full integration tests...
# run integration testing with docker-compose later
CMD ["npm", "run", "int-test"] 


## Stage 5 (default, production)
# this will run by default if you don't include a target
# it has prod-only dependencies
# In BuildKit, this is skipped for dev and test stages
FROM source as prod

CMD ["node", "./bin/www"]

# It's good to make last target the prod stage, because it becomes the default
# build