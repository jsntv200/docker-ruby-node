ARG NODE_VERSION=alpine
ARG RUBY_VERSION=alpine

FROM node:${NODE_VERSION} AS nodejs
FROM ruby:${RUBY_VERSION}

RUN addgroup -g 1000 node \
  && adduser -u 1000 -G node -s /bin/sh -D node \
  && apk add --no-cache libstdc++

COPY --from=nodejs /usr/local/bin/node /usr/local/bin/
COPY --from=nodejs /usr/local/lib/node_modules /usr/local/lib/node_modules
COPY --from=nodejs /opt/ /opt/

RUN ln -sf /usr/local/bin/node /usr/local/bin/nodejs \
  && ln -sf ../lib/node_modules/npm/bin/npm-cli.js /usr/local/bin/npm \
  && ln -sf ../lib/node_modules/npm/bin/npx-cli.js /usr/local/bin/npx \
  && ln -sf /opt/yarn*/bin/yarn /usr/local/bin/yarn \
  && ln -sf /opt/yarn*/bin/yarnpkg /usr/local/bin/yarnpkg

# ARG NODE_VERSION=alpine
# ARG RUBY_VERSION=alpine
#
# FROM node:${NODE_VERSION} AS nodejs
# RUN tar -czf node-dependencies.tar.gz opt usr/local/bin usr/local/lib
#
# FROM ruby:${RUBY_VERSION}
#
# RUN addgroup -g 1000 node \
#   && adduser -u 1000 -G node -s /bin/sh -D node \
#   && apk add --no-cache libstdc++
#
# COPY --from=nodejs node-dependencies.tar.gz .
# RUN tar -zxf node-dependencies.tar.gz
