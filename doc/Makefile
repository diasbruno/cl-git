all: pdf html info

IMAGE_SOURCE =
MAIN_SOURCE = cl-git.texinfo
HTML_IMAGES = ${IMAGE_SOURCE:.dot=.dot.png}

include-stamp: ../*.lisp ../*.asd
	sbcl \
	--eval '(require :sb-texinfo)' \
	--eval '(require :cl-git)' \
	--eval '(sb-texinfo:generate-includes "include/" (list :cl-git) :base-package :cl-git :flatten nil)' \
	--eval '(quit)'
	touch include-stamp


%.html: %.texinfo style.css include-stamp
	makeinfo --html --force --no-split --css-include=style.css $<

%.pdf:  %.texinfo include-stamp
	texi2pdf $<

%.info: %.texinfo include-stamp
	makeinfo --no-split $<


%.neato.pdf: %.dot
	neato -Tpdf -o $@ $<

%.dot.pdf: %.dot
	dot -Tpdf -o $@ $<

%.neato.png: %.dot
	neato -Tpng -o $@ $<

%.dot.png: %.dot
	dot -Tpng -o $@ $<



cl-git.pdf: $(MAIN_SOURCE) ${IMAGE_SOURCE:.dot=.dot.pdf} include-stamp
cl-git.html: $(MAIN_SOURCE) ${IMAGE_SOURCE:.dot=.dot.png} include-stamp

html: clean cl-git.html
pdf: cl-git.pdf
info: cl-git.info


pages: html
	git checkout gh-pages
	cp cl-git.html ../index.html
	-git commit -a -C master
	rm ../index.html
	git checkout -f master

install-info: info
	cp cl-git.info ~/share/info/
	install-info cl-git.info ~/share/info/dir


.PHONY: clean

clean:
	-rm include-stamp
	-rm -rf include/
