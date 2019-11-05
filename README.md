# Blog
A classic blog mostly populated with links towards good articles

[Click here to check it](https://blog.lucas.corsica/)

## Setup

Install nodejs (official binaries https://nodejs.org/en/)

Extract the tar

`tar -xJvf node-$VERSION-$DISTRO.tar.xz`

Create a symbolic link to /usr/local/bin

`ln -s ./node/bin/* /usr/local/bin/`

Install gatsby-cli

`npm install gatsby-cli -g`

Create a symbolic link to /usr/local/bin

`ln -s ./node/bin/* /usr/local/bin/`

Run

`gatsby develop`

## Deploy

To deploy, you just have to push on master

Netlify will build and deploy the blog
