## Dishant.ca

[![Build Status](https://travis-ci.org/dishantlangayan/dishant.ca.svg?branch=master)](https://travis-ci.org/dishantlangayan/dishant.ca)

My personal blog hosted using GitHub pages, Jekyll, and Jasper2 theme at: [dishant.ca](https://dishant.ca)

## Getting Started

### Building and Testing Locally

Ensure you have Jekyll, Bundle installed. Clone the repo and run the following from the repo root directory:

```bash
$ bundle exec jekyll serve
```

Open a browser and navigate to: http://127.0.0.1:4000/

### Deployment

**Important:**  For security reasons, Github does not allow plugins (under `_plugins/`) when
deploying with Github Pages. For this purpose the site is built with [travis-ci](https://travis-ci.org/) (with goodies from
[jekyll-travis](https://github.com/mfenner/jekyll-travis)) automatically pushing the
generated HTML files to a *gh-pages* branch.

### Author Pages

In order to properly generate author pages you need to rename the field *author* in the
front matter of every post to match that of your each author's *username* as defined
in the *[\_data/authors.yml](_data/authors.yml)* file.
With the latest update, multiple author blogs are now supported out of the box.

### Compiling Styles

This blog uses the Jasper2 theme from Ghost, which is based on Casper styles. 

Following on the way Casper styles are compiled as [described here](https://github.com/tryghost/casper#development):

Jasper2 styles are compiled using Gulp/PostCSS to polyfill future CSS spec. You'll need Node and Gulp installed globally. After that, from the theme's root directory:

```bash
$ npm install
$ gulp
```

Now you can edit `/assets/css/` files, which will be compiled to `/assets/built/` automatically.

## Issues and Contributing

This install builds well with Ruby v2.5.1 and Jekyll v3.7.4. If you run into any problems
please log them on the [issue tracker](https://github.com/dishantlangayan/dishant.ca/issues).

Feel free pull-request your patches and fixes.

## Thanks

Many thanks to the Ghost team for all the design work. Thanks for Unsplash.com for images used in this blog.

## Copyright & License

Copyright (C) 2019-2020 Dishant Langayan - Released under the MIT License.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
