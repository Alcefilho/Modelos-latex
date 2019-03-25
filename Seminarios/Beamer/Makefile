
.PHONY: all clean structure pdfs

THEMES=$(basename $(subst /usr/share/texmf/tex/latex/beamer/themes/theme/beamertheme,,$(wildcard /usr/share/texmf/tex/latex/beamer/themes/theme/*.sty)))
COLORS=$(basename $(subst /usr/share/texmf/tex/latex/beamer/themes/color/beamercolortheme,,$(wildcard /usr/share/texmf/tex/latex/beamer/themes/color/*.sty)))
FONTS=$(basename $(subst /usr/share/texmf/tex/latex/beamer/themes/font/beamerfonttheme,,$(wildcard /usr/share/texmf/tex/latex/beamer/themes/font/*.sty)))

COMBINATIONS=$(foreach t,$(THEMES),$(foreach c,$(COLORS),$(foreach f,$(FONTS),$(t)-$(c)-$(f))))

PDFS=$(addsuffix .pdf,$(COMBINATIONS))
TEXS=$(addsuffix .tex,$(COMBINATIONS))

IMAGES=$(patsubst %,merged/%.jpg,$(COMBINATIONS)) \
$(foreach c, $(COMBINATIONS), $(foreach size, large small, $(foreach page, 01 02 12 17 18, $(size)/$(c)-$(page).png)))

all: structure $(IMAGES)

pdfs: $(PDFS)

texs: $(TEXS)

structure: large small merged

large small merged:
	mkdir $@

large/%-01.ppm large/%-02.ppm: %.pdf
	pdftoppm -f  1 -l  2 "$<" large/$(basename $<)

large/%-12.ppm: %.pdf
	pdftoppm -f 12 -l 12 "$<" large/$(basename $<)

large/%-17.ppm large/%-18.ppm: %.pdf
	pdftoppm -f 17 -l 18 "$<" large/$(basename $<)


merged/%.jpg: large/%-01.ppm large/%-02.ppm large/%-12.ppm large/%-17.ppm large/%-18.ppm
	pnmcat -lr $^ | pnmscale 0.142857143 | pnmtojpeg --quality=85 --optimize >  "$@"

small/%.png: large/%.ppm
	pnmscale 0.428571429 < "$<" | pnmtopng -compression 3 > "$@"

large/%.png: large/%.ppm
	pnmtopng -compression 3 < "$<" > "$@"

%.tex: template.tex.tmpl
	@head -n 23 "$<" > "$@"
	@echo "$@" | sed 's/\([^-]*\)-\([^-]*\)-\([^-\.]*\)\..*/\\usetheme{\1}\n\\usecolortheme{\2}\n\\usefonttheme{\3}\n/' >> "$@"
	@tail -n 716 "$<" >> "$@"
	@touch -r template.tex.tmpl "$@"

%.pdf: %.tex 
	latexmk -quiet -pdf "$<"
	rm $(addprefix $(basename $<)., aux log nav out snm toc aux.bak)

clean:
	rm *.aux *.log *.nav *.out *.snm *.toc *.aux.bak
