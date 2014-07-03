## About ##
The Little Redis Book is a free book introducing Redis.

The book was written by [Karl Seguin](http://openmymind.net), with [Perry Neal](http://twitter.com/perryneal)'s assistance.

If you liked this book, maybe you'll also like [The Little MongoDB Book](http://openmymind.net/2011/3/28/The-Little-MongoDB-Book/).

## License ##
The book is freely distributed under the  [Attribution-NonCommercial 3.0 Unported license](<http://creativecommons.org/licenses/by-nc/3.0/legalcode>).

## Translations ##

* [Russian](https://github.com/kondratovich/the-little-redis-book)
* [Italian](https://github.com/sandroconforto/the-little-redis-book) - [pdf](https://github.com/sandroconforto/the-little-redis-book/raw/master/book/redisIt.pdf)
* [Chinese](https://github.com/JasonLai256/the-little-redis-book)
* [Chinese 2](https://github.com/geminiyellow/the-little-redis-book/)
* [Japanese](https://github.com/craftgear/the-little-redis-book/)
* [Spanish](https://github.com/raulexposito/the-little-redis-book)

## Formats ##
The book is written in [Markdown](http://daringfireball.net/projects/markdown/) and converted to PDF using [Pandoc](http://johnmacfarlane.net/pandoc/).

The TeX template makes use of [Lena Herrmann's JavaScript highlighter](http://lenaherrmann.net/2010/05/20/javascript-syntax-highlighting-in-the-latex-listings-package).

Kindle and ePub format provided using [Pandoc](http://johnmacfarlane.net/pandoc/).

## Generating books ##
Packages listed below are for Ubuntu. If you use another OS or distribution names would be similar.

### PDF

#### Dependencies

Packages:

* `pandoc`
* `texlive-xetex`
* `texlive-latex-extra`
* `texlive-latex-recommended`

You should have [some fonts](https://github.com/karlseguin/the-little-redis-book/blob/master/common/pdf-template.tex#L11) installed too.
Or you could change them to other ones if you want. Consider that fonts could cause [building troubles](https://github.com/karlseguin/the-little-redis-book/issues/26).

#### Building

Run `make en/redis.pdf`.

### ePub

#### Dependencies

Packages:

* `pandoc`

#### Building

Run `make en/redis.epub`.

### Mobi

#### Dependencies

Packages:

* `pandoc`

You should have [KindleGen](http://www.amazon.com/gp/feature.html?ie=UTF8&docId=1000765211) installed too.

#### Building

Run `make en/redis.mobi`.

## Title Image ##
A PSD of the title image is included. The font used is [Comfortaa](http://www.dafont.com/comfortaa.font).
