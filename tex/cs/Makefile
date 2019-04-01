export TEXINPUTS=../tex//:

all: prace.pdf abstrakt.pdf

# LaTeX je potreba spustit nekolikrat, aby spravne spocital odkazy
prace.pdf: prace.tex $(wildcard *.tex) library.bib prace.xmpdata
	mkdir -p output
	pdflatex -output-directory output $<
	cp *.bib output
	cp *.bst output
	cd output; bibtex prace
	pdflatex -output-directory output $<
	pdflatex -output-directory output $<

abstrakt.pdf: abstrakt.tex abstrakt.xmpdata
	pdflatex -output-directory output $<

clean:
	rm -f *.log *.dvi *.aux *.toc *.lof *.lot *.out *.bbl *.blg *.xmpi
	cd output; rm -f *.log *.dvi *.aux *.toc *.lof *.lot *.out *.bbl *.blg *.xmpi
	rm -f prace.pdf abstrakt.pdf
