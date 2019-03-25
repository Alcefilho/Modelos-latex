NOME=Notas_de_Aula
USER=$(shell pwd | cut -f1-3 -d/ )
HOME=$(shell pwd | sed -r 's|(^.*)\/.*|\1|' )
LOCAL=$(shell pwd)
DIR=$(shell pwd | rev | cut -f1 -d "/" | rev)
YEAR=$(shell date +%Y-%m-%d)
SAIDA=Notas_de_Aula_de_MecQuant_$(YEAR)
OPT=-shell-escape 
OPT2=-shell-restricted --enable-write18
Lista_In=*.aux
#VIEWER=/usr/bin/epdfview
#VIEWER=/usr/bin/acroread
#VIEWER=/usr/bin/kghostview
#VIEWER=/usr/bin/xpdf
VIEWER=evince

all:	dvi pspdf Matar clean view

#all:	dvi dvipdf Matar clean view

#all:	pdftex Matar clean view

dvi:
	latex $(OPT) $(NOME) ;\
	latex $(OPT) $(NOME) ;\
	for arqv in *.aux ;\
	do \
	   echo $$arqv ;\
	   bibtex $$arqv ;\
	done ;\
	latex $(OPT) $(NOME) ;\
	latex $(OPT) $(NOME)
#		makeindex $(NOME).idx ;\
#		makeindex -s  /home/ldp/estilos/indice.ist $(NOME).idx ;\
#		latex $(NOME) ;\
#		latex $(NOME)

BIB:
	for arqv in *.aux ;\
	do \
	   echo $$arqv ;\
	   bibtex $$arqv ;\
	done 

pdftex:
	rm -f .aux .log .out $(NOME).dvi $(NOME).pdf ;\
	pdflatex $(OPT) $(NOME) ;\
	pdflatex $(OPT) $(NOME) ;\
	for arqv in *.aux ; do \
	   bibtex $$arqv ;\
	done ;\
	mv $(NOME).pdf $(SAIDA).pdf 	

pdf:
	if [ -f $(NOME).dvi ] ;\
	then dvipdfm -o $(NOME).pdf -l -p a4 $(NOME).dvi;\
	fi ; \
	if [ -f $(NOME).pdf ] ;\
	then ppower4 $(NOME).pdf $(SAIDA).pdf;\
	fi

pspdf:
	if [ -f $(NOME).dvi ] ;\
	then dvips -R0 -Z -o $(NOME).ps $(NOME).dvi;\
	ps2pdf $(NOME).ps $(SAIDA).pdf ;\
	fi

print:
	latex Semi_Print ;\
	latex Semi_Print ;\
	if [ -f Semi_Print.dvi ] ;\
	then dvipdfm -o Semi_Print.pdf -p a4 Semi_Print.dvi;\
	fi
	rm -f *.aux *.soc *.toc *.lo[fpt] *.blg *.bbl *.log *.out \
	*.ind *.ilg *.idx *.glo *.gls ;\
	/usr/bin/acroread $(HOME)/$(LOCAL)/Semi_Print.pdf &

dvipdf:
	if [ -f $(NOME).dvi ] ;\
	then dvipdfmx -o $(NOME).pdf $(NOME).dvi;\
	mv $(NOME).pdf $(SAIDA).pdf ;\
	fi

Matar:
	$(shell ps aux | grep $(SAIDA).pdf | tr -s " " | cut -d" " -f2 | xargs kill -9 )

view:
	$(shell $(VIEWER) $(SAIDA).pdf)

clean:
	rm -rf  *.aux *.soc *.toc *.lo[fpt] *.blg *.bbl *.log *.out \
	*.ind *.ilg *.idx *.glo *.gls *.$(NOME) *.bm *.ps *.vbr *.backup

backup:
	make clean ;\
	cd $(HOME) ; if [ -f $(HOME)/$(NOME).tar.gz ] ;\
	then echo "Deletando" $(HOME)/$(NOME).tar.gz ;\
	rm -f $(HOME)/$(NOME).tar.gz; fi ;\
	tar -zcf $(NOME).tar.gz $(DIR)/* ;\
	echo "Backup gerado com sucesso";\
	ls -alh $(HOME)/$(NOME).tar.gz 2> /dev/null
