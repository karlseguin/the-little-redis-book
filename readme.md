## About ##
The Little Redis Book is a free book introducing Redis.

The book was written by [Karl Seguin](http://openmymind.net), with [Perry Neal](http://twitter.com/perryneal)'s assistance.

If you liked this book, maybe you'll also like [The Little MongoDB Book](http://openmymind.net/2011/3/28/The-Little-MongoDB-Book/).

## License ##
The book is freely distributed under the  [Attribution-NonCommercial 3.0 Unported license](<http://creativecommons.org/licenses/by-nc/3.0/legalcode>).

## Translations ##

* [Russian](https://github.com/kondratovich/the-little-redis-book)

## Formats ##
The book is written in [Markdown](http://daringfireball.net/projects/markdown/) and converted to PDF using [pandoc](http://johnmacfarlane.net/pandoc/). A few LaTex specific commands have been placed in the Markdown file to help with PDF-generation (namely for the title page and to create page breaks between chapters).

To generate PDF, Kindle and EPUB formats, download and install [pandoc](http://johnmacfarlane.net/pandoc/), a universal document converter.

## Generating the PDF ##
pandoc includes markdown2pdf to generate the PDF using a variation of <https://github.com/claes/pandoc-templates>:

	#!/bin/sh
	paper=a4paper
	hmargin=3cm
	vmargin=3cm
	fontsize=11pt

	mainfont=Verdana
	sansfont=Tahoma
	monofont="Courier New"
	columns=onecolumn
	geometry=portrait
	nohyphenation=true


	markdown2pdf --xetex --template=template/xetex.template \
	-V paper=$paper -V hmargin=$hmargin -V vmargin=$vmargin \
	-V mainfont="$mainfont" -V sansfont="$sansfont" -V monofont="$monofont" \
	-V geometry=$geometry -V columns=$columns -V fontsize=$fontsize \
	-V nohyphenation=$nohyphenation en/redis.md -o redis.pdf

## Generating the EPUB ##
Use the following command (modified from the one found at <http://news.ycombinator.com/item?id=3502033>) to generate the EPUB:

	pandoc -f markdown -t epub --epub-metadata=en/metadata.xml \
	--template=template/xetex.template -V paper=$paper \
	-V hmargin=$hmargin -V vmargin=$vmargin -V mainfont="$mainfont" \
	-V sansfont="$sansfont" -V monofont="$monofont" -V geometry=$geometry \
	-V columns=$columns -V fontsize=$fontsize -V nohyphenation=$nohyphenation \
	en/redis.md -o redis.epub

## Title Image ##
A PSD of the title image is included. The font used is [Comfortaa](http://www.dafont.com/comfortaa.font).
