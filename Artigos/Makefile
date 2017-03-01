#NOME=Modelo_Artigo_CPL
#NOME=Modelo_Artigo_simples
NOME=Modelo_Artigo_revtex
#SAIDA=Paper
LOCAL=$(shell pwd )
HOME=$(shell dirname $(shell pwd) )
#HOME=$(shell pwd | sed -r 's|(^.*)\/.*|\1|' )
DIR=$(shell basename $(shell pwd) )
#DIR=$(shell pwd | rev | cut -f1 -d "/" | rev)
USER=$(shell pwd | cut -f1-3 -d/ )
YEAR=$(shell date +%Y)
DATA=$(shell date +%d_%m_%Y)
ARQS=$(shell egrep "[\]author" $(NOME).tex | sed 's/^.*{\(.*\)}/\1/g; s/ /_/g')
ARQT=$(shell egrep "[\]title" $(NOME).tex | sed 's/^.*{\(.*\)}/\1/g; s/ /_/g')
#SAIDA=Artigo_$(ARQ)_$(YEAR)
#SAIDA=$(ARQT)_$(DATA)
#SAIDA=$(NOME)_$(DATA)
SAIDA=$(NOME)
BACKUP=$(HOME)/$(NOME)_$(DATA).tar.gz
#VIEWER=acroread
#VIEWER=kghostview
#VIEWER=xpdf
#VIEWER=okular
VIEWER=evince

all:	dvi pspdf view

dvi:
	rm -f .aux .log .out $(NOME).dvi $(NOME).pdf ;\
	latex $(NOME) ;\
	latex $(NOME) ;\
	bibtex $(NOME) ;\
	latex $(NOME) ;\
	latex $(NOME)
#		makeindex $(NOME).idx ;\
#		makeindex -s  /home/ldp/estilos/indice.ist $(NOME).idx ;\
#		latex $(NOME) ;\
#		latex $(NOME)


pdf:
	if [ -f $(NOME).dvi ] ;\
	then dvipdfmx -o $(NOME).pdf -p a4 $(NOME).dvi;\
	fi ; \
	make clean

pspdf:
	if [ -f $(NOME).dvi ] ;\
	then dvips -Ppdf -G0 -o $(NOME).ps $(NOME).dvi;\
	ps2pdf $(NOME).ps $(SAIDA).pdf ;\
	fi
#	make clean

print:
	latex Semi_Print ;\
	latex Semi_Print ;\
	if [ -f Semi_Print.dvi ] ;\
	then dvipdfm -o Semi_Print.pdf -p a4 Semi_Print.dvi;\
	fi
	rm -f *.aux *.soc *.toc *.lo[fpt] *.blg *.bbl *.log *.out \
	*.ind *.ilg *.idx *.glo *.gls ;\
	$(shell $(VIEWER) $(HOME)/$(LOCAL)/Semi_Print.pdf )&

Matar:
	$(shell ps aux | grep $(SAIDA).pdf | cut -d" " -f2 | xargs kill -9)

view:
	$(shell $(VIEWER) $(SAIDA).pdf) &

edit:
	$(shell kile $(HOME)/$(LOCAL)/$(NOME).tex $(HOME)/$(LOCAL)/$(SAIDA).tex) &

clean:
	rm -rf  *.aux *.soc *.toc *.lo[fpt] *.blg *.bbl *.log *.out \
	*.ind *.ilg *.idx *.glo *.gls *.$(NOME) $(NOME).dvi *~ *.backup \
	$(NOME).ps

backup:
	cd $(HOME) ; if [ -f $(BACKUP) ] ;\
	then @echo "Deletando" $(BACKUP) ;\
	rm -f $(BACKUP); fi ;\
	tar -zcf $(BACKUP) $(LOCAL)/* ;\
	echo "Backup gerado com sucesso";\
	ls -alh $(BACKUP) 2> /dev/null

