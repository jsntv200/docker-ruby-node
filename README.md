# Ruby + Node Docker Image

Docker multistage image with Ruby and Node.js.

This image uses the Ruby and Node.js official images with Ruby as the base repository.

Any supported version of Ruby or Node can be installed by passing a `--build-arg`
for `RUBY_VERSION` &/or `NODE_VERSION`.

- [Docker Ruby](https://hub.docker.com/_/ruby/)
- [Docker Node](https://hub.docker.com/_/node/)

If no `--build-arg` is passed then each variant will default to `alpine` or `stretch`.

## Differences with official Ruby and Node.js images?

Ruby: Same as official.

Node: Same as official except the `NODE_MAJOR` & `NODE_VERSION` env vars are not
set. If required just set them in your Dockerfile.

```
ENV NODE_MAJOR 10
ENV NODE_VERSION 10.15.1
```

Since Ruby is the base image `irb` is the default CMD.

## Image Variants

The `red-amt/ruby-node` images come in two flavours.

`red-ant/ruby-node:stretch`

This is the defacto image. If you are unsure about what your needs are, you
probably want to use this one. It is designed to be used both as a throw away
container (mount your source code and start the container to start your app), as
well as the base to build other images off of. Based on Debian distribution.

`red-ant/ruby-node:alpine`

This is the smallest image possible. It is based on the Alpine Linux base image.

## Default Locale

The default locale is set to C.UTF-8 instead default POSIX.

## Supported Docker versions

Since this image supports multistage and ARG before FROM it only supports Docker
versions >17.05.

## Example Dockerfile

A Dockerfile to build a rails app with webpacker, postgres and some gems loaded
via a git url would look like:

```
FROM red-ant/ruby-node:alpine

RUN apk add --no-cache --virtual .build-deps \
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

And the build command if you wanted to pin to specific versions would be:

```
docker build -t redant:alpine-2-10 . \
  --build-arg RUBY_VERSION=2.5.1
  --build-arg NODE_VERSION=10.15.0
```


### Documentation

- [Docker](http://docs.docker.com)
- [Ruby](https://www.ruby-lang.org/en/)
- [Node.js](https://nodejs.org/en/)
