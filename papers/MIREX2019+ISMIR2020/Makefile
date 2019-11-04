export TEXINPUTS=../tex//:

all: ISMIR2019template.pdf

# LaTeX je potreba spustit nekolikrat, aby spravne spocital odkazy
ISMIR2019template.pdf: ISMIR2019template.tex $(wildcard *.tex) library.bib
	pdflatex ISMIR2019template.tex
	bibtex ISMIR2019template
	pdflatex ISMIR2019template.tex
	pdflatex ISMIR2019template.tex

clean:
	rm -f *.log *.dvi *.aux *.toc *.lof *.lot *.out *.bbl *.blg *.xmpi
	cd output; rm -f *.log *.dvi *.aux *.toc *.lof *.lot *.out *.bbl *.blg *.xmpi
	cd output; rm -f *.pdf