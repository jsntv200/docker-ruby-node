# Multistage Alpine Ruby + Node.js

A multistage docker image using the official Docker Hub Ruby and Node.js builds.
The image contains pre-installed versions of `ruby`, `bundler`, `node`, `npm`
and `yarn`.

The default entrypoint is `bash`.

## Default Versions
- [Ruby 2.5](https://github.com/docker-library/ruby/blob/master/2.5/alpine3.9/Dockerfile)
- [Node 10.15](https://github.com/nodejs/docker-node/blob/master/10/alpine/Dockerfile)

## Supported Build Args
 - RUBY_VERSION
 - NODE_VERSION

Build an image using Ruby 2.6 & Node 11:

    docker build -t my:tag github.com/jsntv200/docker-ruby-node \
      --build-arg RUBY_VERSION=2.6
      --build-arg NODE_VERSION=11

## Usage

    docker run -it my:tag

#### Run `ruby`

    docker run -it my:tag ruby

#### Run `node`

    docker run -it my:tag node
