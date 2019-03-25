#
# Makefile 
#
PDFLATEX = pdflatex
FILE = beamer
FILE2 = hello-bmr
FILE3 = beamer-final

all:
	${PDFLATEX} ${FILE}.tex
	${PDFLATEX} ${FILE2}.tex
	${PDFLATEX} ${FILE3}.tex

clean:
	@echo "Cleaning up..."
	-rm -rvf *~ \#*
	-rm -rvf ._*
	-rm -rvf .~*

distclean:
	@echo "Cleaning all..."
	-rm -rvf *~ \#*
	-rm -rvf ${FILE}.pdf
	-rm -rvf ${FILE2}.pdf
	-rm -rvf ${FILE3}.pdf
	-rm -rvf *.aux 
	-rm -rvf *.log *.vrb *.toc *.nav *.snm *.out

