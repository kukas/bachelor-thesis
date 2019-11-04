export TEXINPUTS=../tex//:

all: ISMIRlbd.pdf

# LaTeX je potreba spustit nekolikrat, aby spravne spocital odkazy
ISMIRlbd.pdf: ISMIRlbd.tex $(wildcard *.tex) library.bib
	pdflatex ISMIRlbd.tex
	bibtex ISMIRlbd
	pdflatex ISMIRlbd.tex
	pdflatex ISMIRlbd.tex

clean:
	rm -f *.log *.dvi *.aux *.toc *.lof *.lot *.out *.bbl *.blg *.xmpi
	cd output; rm -f *.log *.dvi *.aux *.toc *.lof *.lot *.out *.bbl *.blg *.xmpi
	cd output; rm -f *.pdf