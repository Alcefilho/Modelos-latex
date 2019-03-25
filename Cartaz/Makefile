NOME=Seminario
HOME=$(shell pwd | sed -r 's|(^.*)\/.*|\1|' )
USER=$(shell pwd | cut -f1-3 -d/ )
LOCAL=$(shell pwd)
DIR=$(shell pwd | rev | cut -f1 -d "/" | rev)
YEAR=$(shell date +%d_%m_%Y)
SAIDA=Saida$(YEAR)
#VIEWER=kghostview
#VIEWER=xpdf
#VIEWER=acroread
#VIEWER=okular
VIEWER=evince

all:	dvi pspdf view

dvi:
	rm -f .aux .log .out $(NOME).dvi $(NOME).pdf ;\
	latex $(NOME) ;\
	latex $(NOME) 
#		makeindex $(NOME).idx ;\
#		makeindex -s  /home/ldp/estilos/indice.ist $(NOME).idx ;\
#		latex $(NOME) ;\
#		latex $(NOME)


pdf:
	if [ -f $(NOME).dvi ] ;\
	then dvipdfm -o $(NOME).pdf -l -p a4 $(NOME).dvi;\
	fi ; \
	if [ -f $(NOME).pdf ] ;\
	then ppower4 $(NOME).pdf $(SAIDA).pdf;\
	fi ; \
	make clean

pspdf:
	if [ -f $(NOME).dvi ] ;\
	then dvips -o $(NOME).ps $(NOME).dvi;\
        ps2pdf $(NOME).ps $(SAIDA).pdf ;\
	fi ; \
	make clean


print:
	latex Semi_Print ;\
	latex Semi_Print ;\
	if [ -f Semi_Print.dvi ] ;\
	then dvipdfm -o Semi_Print.pdf -p a4 Semi_Print.dvi;\
	fi
	rm -f *.aux *.soc *.toc *.lo[fpt] *.blg *.bbl *.log *.out \
	*.ind *.ilg *.idx *.glo *.gls ;\
	/usr/bin/acroread $(HOME)/$(LOCAL)/Semi_Print.pdf &

Matar:
	$(shell ps aux | grep $(SAIDA).pdf | cut -d" " -f2 | xargs kill -9)

view:
	$(shell $(VIEWER) $(SAIDA).pdf) &
	
clean:
	rm -rf  *.aux *.soc *.toc *.lo[fpt] *.blg *.bbl *.log *.out \
	*.ind *.ilg *.idx *.glo *.gls *.$(NOME) *.bm *.ps *.vbr *.backup

backup:
	cd $(HOME) ; if [ -f $(HOME)/$(NOME).tar.gz ] ;\
	then echo "Deletando" $(HOME)/$(NOME).tar.gz ;\
	rm -f $(HOME)/$(NOME).tar.gz; fi ;\
	tar -cf $(NOME).tar $(LOCAL)/* ;\
	gzip $(NOME).tar ;\
	echo "Backup gerado com sucesso";\
	ls -alh $(HOME)/$(NOME).tar.gz
