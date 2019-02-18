# Multistage Ruby + Node

A multistage build using the official Ruby and Node.js docker images.

Any supported version of Ruby or Node can be installed by passing a `--build-arg`
for `RUBY_VERSION` &/or `NODE_VERSION`.

- [Docker Ruby](https://hub.docker.com/_/ruby/)
- [Docker Node](https://hub.docker.com/_/node/)

If no `--build-arg` is passed then each variant will default to `alpine` or `latest`
for the debian buid.

## Differences with official Ruby and Node.js images?

Ruby: Same as official.

Node: Same as official except the `NODE_MAJOR` & `NODE_VERSION` env vars are not
set. If required just set them in your Dockerfile.

```
ENV NODE_MAJOR 10
ENV NODE_VERSION 10.15.1
```

Ruby is the base image so `irb` is the default CMD.

## Image Variants

The `red-amt/ruby-node` images come in two flavours.

`red-ant/ruby-node:debian`

Based on the Debian distribution it will work with any of the Debian based tags.

`red-ant/ruby-node:alpine`

Based on the Alpine distribution it will produce the smallest image possible.

## Default Locale

The default locale is set to C.UTF-8 instead default POSIX for the Debian builds.

## Supported Docker versions

Since this image supports multistage and uses ARG before FROM it only supports
Docker versions `>= 17.05`.

## Example Dockerfile

A Dockerfile to build a rails app with support for postgres and gems loaded via
a git url with Alpine as the base:

```
FROM red-ant/ruby-node:alpine

RUN apk add --virtual .build-deps \
  build-base \
  git \
  postgresql-dev

WORKDIR /app

COPY Gemfile* ./
RUN bundle install

COPY package.json yarn.lock ./
RUN yarn install

COPY . ./
RUN bundle exec rake assets:precompile
RUN apk del .build-deps

EXPOSE 3000
```

Build the image using latest versions :

```
docker build -t app:alpine .
```

Build the image with specific versions :

```
docker build -t app:alpine-2-10 . \
  --build-arg RUBY_VERSION=2.5.1 \
  --build-arg NODE_VERSION=10.15.0
```

### Documentation

- [Docker](http://docs.docker.com)
- [Ruby](https://www.ruby-lang.org/en/)
- [Node.js](https://nodejs.org/en/)
