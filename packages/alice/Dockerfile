FROM node:10.13-alpine as builder

# Environment

WORKDIR /home/app
ENV NODE_ENV=production

# Dependencies

COPY package.json /home/app/
COPY package-lock.json /home/app/
COPY lerna.json /home/app/

COPY packages/alice/package.json /home/app/packages/alice/
COPY packages/alice/package-lock.json /home/app/packages/alice/

COPY packages/common/package.json /home/app/packages/common/
COPY packages/common/package-lock.json /home/app/packages/common/

RUN npm ci --ignore-scripts --production --no-optional
RUN npx lerna bootstrap --hoist --ignore-scripts -- --production --no-optional

# Build

COPY . /home/app/
RUN cd packages/alice && npm run build

# Serve

FROM nginx:1.15-alpine
COPY --from=builder /home/app/packages/alice/build /usr/share/nginx/html
