FROM node:10

# set this with shell variables at build-time.
# If they aren't set, then not-set will be default.
ARG CREATED_DATE=not-set
ARG SOURCE_COMMIT=not-set

# Starting LABELS with org.opencontainers.image is standard now

# labels from https://github.com/opencontainers/image-spec/blob/master/annotations.md
LABEL org.opencontainers.image.authors=bret@bretfisher.com
LABEL org.opencontainers.image.created=$CREATED_DATE
LABEL org.opencontainers.image.revision=$SOURCE_COMMIT
LABEL org.opencontainers.image.title="Sample Node.js Dockerfile with LABELS"
LABEL org.opencontainers.image.url=https://hub.docker.com/r/bretfisher/jekyll
LABEL org.opencontainers.image.source=https://github.com/BretFisher/udemy-docker-mastery-for-nodejs
LABEL org.opencontainers.image.licenses=MIT

# Custom labels example. Put domain name in reverse format. Can populate
# labels with env vars as u c here. Labels are for production
LABEL com.bretfisher.nodeversion=$NODE_VERSION

WORKDIR /app

COPY index.js .

CMD ["node", "index.js"]

# Notes on adding test stages
# Add a separate test stage that doens't depend on anything
# Make it not the default, a one-off stage
# Run "docker-compose run node npm test" locally, don't try to run / rebuild the
#  entire image 