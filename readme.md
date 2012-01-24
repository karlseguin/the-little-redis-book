Форк исходных текстов книги "The Little Redis Book" для перевода на русский язык.

В папке ru лежит копия английской версии для редактирования. Желающие помочь — присылайте пулл реквесты. Гитхаб позволяет редактировать документ прямо с веб-страницы.

## About ##
The Little Redis Book is a free book introducing Redis.

The book was written by [Karl Seguin](http://openmymind.net), with [Perry Neal](http://twitter.com/perryneal)'s assistance. 

If you liked this book, maybe you'll also like [The Little MongoDB Book](http://openmymind.net/2011/3/28/The-Little-MongoDB-Book/).

## License ##
The book is freely distributed under the  [Attribution-NonCommercial 3.0 Unported license](<http://creativecommons.org/licenses/by-nc/3.0/legalcode>).

## Formats ##
The book is written in [markdown](http://daringfireball.net/projects/markdown/) and converted to PDF using [PanDoc](http://johnmacfarlane.net/pandoc/). A few LaTex specific commands have been placed in the markdown file to help with pdf-generation (namely for the title page and to create page breaks between chapters).

Kindle and ePub format provided using [PanDoc](http://johnmacfarlane.net/pandoc/).

## Generating the PDF ##
I use a variation of <https://github.com/claes/pandoc-templates> to generate the pdf:

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

## Title Image ##
A PSD of the title image is included. The font used is [Comfortaa](http://www.dafont.com/comfortaa.font).
