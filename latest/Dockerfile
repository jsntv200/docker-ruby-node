ARG NODE_VERSION=latest
ARG RUBY_VERSION=latest

FROM node:$NODE_VERSION AS nodejs
RUN tar -czf nodejs-deps.tar.gz \
  opt \
  usr/local/bin \
  usr/local/lib

FROM ruby:$RUBY_VERSION
ENV LANG C.UTF-8

RUN groupadd --gid 1000 node \
  && useradd --uid 1000 --gid node --shell /bin/bash --create-home node

COPY --from=nodejs nodejs-deps.tar.gz nodejs-deps.tar.gz
RUN tar -xzf nodejs-deps.tar.gz && rm nodejs-deps.tar.gz
