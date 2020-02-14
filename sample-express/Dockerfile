FROM node:10.15-slim

ENV NODE_ENV=production

# Start in parent directory...
WORKDIR /node

COPY package.json package-lock*.json ./

# Install in parent directory
RUN npm install && npm cache clean --force

WORKDIR /node/app

COPY . .

CMD ["node", "./bin/www"]
