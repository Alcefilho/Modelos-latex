NOME=Seminario
LOCAL=$(shell pwd )
HOME=$(shell dirname $(shell pwd) )
YEAR=$(shell date +%Y)
DATA=$(shell date +%d_%m_%Y)
SAIDA=MeuSlide_$(DATA)
BACKUP=$(HOME)/$(NOME)_$(DATA).tar.gz
VIEWER=evince
#VIEWER=/usr/bin/acroread
#VIEWER=/usr/bin/okular
#VIEWER=/usr/bin/xpdf

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

view:
	$(shell $(VIEWER) $(SAIDA).pdf) &
	
clean:
	rm -rf  *.aux *.soc *.toc *.lo[fpt] *.blg *.bbl *.log *.out \
	*.ind *.ilg *.idx *.glo *.gls *.$(NOME) *.bm *.ps *.vbr *.backup

backup:
	cd $(HOME) ; if [ -f $(BACKUP) ] ;\
	then @echo "Deletando" $(BACKUP) ;\
	rm -f $(BACKUP); fi ;\
	tar -zcf $(BACKUP) $(LOCAL)/* ;\
	echo "Backup gerado com sucesso";\
	ls -alh $(BACKUP) 2> /dev/null
