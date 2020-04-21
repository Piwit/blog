# Blog
My own personal blog, based on the gatsby blog template

Articles are written in Markdown in the content folder

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
