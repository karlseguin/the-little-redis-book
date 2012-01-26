Форк исходных текстов книги "The Little Redis Book" для перевода на русский язык.

В папке ru лежит копия английской версии для редактирования. Желающие помочь — присылайте пулл реквесты. Гитхаб позволяет редактировать документ прямо с веб-страницы.

## О Книге ##
The Little Redis Book - это бесплатная книга про Redis.

Книга была написана [Karl Seguin](http://openmymind.net), при поддержке [Perry Neal](http://twitter.com/perryneal).
Если вам нравится эта книга, вам также стоит обратить внимание на [The Little MongoDB Book](http://openmymind.net/2011/3/28/The-Little-MongoDB-Book/).

## Лицензия ##
Книга свободно роспространяется под [Attribution-NonCommercial 3.0 Unported license](<http://creativecommons.org/licenses/by-nc/3.0/legalcode>).

## Форматы ##
Книга написана с пощью [markdown](http://daringfireball.net/projects/markdown/) и сконвертирована в PDF при помощи [PanDoc](http://johnmacfarlane.net/pandoc/). Несколько комманд LaTeX были добавлены для генерации PDF (для титульной страницы и для создания разрывов страниц между главами).

Kindle и ePub форматы можно сгенерировать с помощью [PanDoc](http://johnmacfarlane.net/pandoc/).

## Generating the PDF ##
pandoc includes markdown2pdf to generate the PDF using using a variation of <https://github.com/claes/pandoc-templates>:

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
