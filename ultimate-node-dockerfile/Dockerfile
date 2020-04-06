# Stage 1: Base stage. Set labels, ports, build args, and install production
# dependencies
FROM node:8-slim AS base

# These can be set at build time with --build-arg. These aren't persisted in
# the container, so they can contain secrets! See
# https://docs.docker.com/engine/reference/commandline/build/#set-build-time-variables---build-arg#set-build-time-variables---build-arg
ARG CREATED_DATE=not-set
ARG SOURCE_COMMIT=not-set

LABEL org.opencontainers.image.created=${CREATED_DATE}
LABEL org.opencontainers.image.authors=Me
LABEL org.opencontainers.image.url=https://blepper.andrewray.me
LABEL org.opencontainers.image.documentation=https://blepper.andrewray.me
LABEL org.opencontainers.image.source=https://blepper.andrewray.me
LABEL org.opencontainers.image.version=${SOURCE_COMMIT}
LABEL org.opencontainers.image.revision=${SOURCE_COMMIT}
LABEL org.opencontainers.image.vendor=wtf
LABEL org.opencontainers.image.licenses=mit
LABEL org.opencontainers.image.title="Hi how are you"
LABEL org.opencontainers.image.description="I am descripkptkion"
LABEL com.andyray.nodeversion=${NODE_VERSION}

# Note assignment didn't call for USER node, but it should have

EXPOSE 80

ENV NODE_ENV production

WORKDIR /app

COPY package*.json ./

# Npm ci, relatively new, uses package-lock.json every time
# When buildig the image, add a helpful command to give debugging output
RUN npm config list \
    && npm ci \
    && npm cache clean --force

# Stage 2: Development.
# Note we don't copy here, because this is bind mounted
FROM base AS dev

# Needed to install phantomjs devdependency
RUN apt-get update -qq && apt-get install -qy \
    ca-certificates \
    bzip2 \
    curl \
    libfontconfig \
    --no-install-recommends

ENV NODE_ENV=development

ENV PATH /app/node_modules/.bin:$PATH

RUN npm config list \
    && npm install --only=development

CMD ["nodemon", "server.js", "--inspect=0.0.0.0:9229"]

# Stage 3: Test
# For CI test running only, unit tests only, not e2e. This stage also should
# not be depended on by any other stages
FROM dev AS test

# A copy command shouldn't be at the top of the Dockerfile, because
# changing any file in source will bust whole cache
COPY . .

CMD ["npm", "test"]

# Stage 4: Audit
# Don't need to do this stage if code isn't correct, so 
FROM test AS audit

RUN npm audit

# Stage 5: clean up
# Remove files in this stage so the production image will be as
# small as possible
FROM test AS pre-prod
RUN rm -rf ./tests ./node_modules

# Stage 6: Production
# Production! Last stage is always production stage, so if you forget to target
# a stage, this works by default
FROM base AS prod

# Copy with --from doesn't obey working directory
COPY --from=pre-prod /app /app

HEALTHCHECK --interval=30s --timeout=30s --start-period=5s --retries=3 \
    CMD curl -f http://localhost || exit 1

CMD ["node", "server.js"]
