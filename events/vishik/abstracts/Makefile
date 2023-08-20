all: abstracts pdf permissions

abstracts: abstracts-a5.pdf permissions

pdf:
	for i in *.tex; do make `echo $$i| sed 's/.tex/.pdf/'`; done

permissions:
	chmod a+r *.pdf


%.dvi: %.tex
	latex $<
	latex $<

%.pdf: %.dvi
	dvi2pdf.a4 $<

clean:
	rm *~

abstracts.tex: *.tex abstracts-top abstracts-bottom Makefile 
	echo '\documentclass[9pt,a5paper]{extarticle}' > abstracts.tex
	chmod a+r abstracts.tex
	echo '\def\version'"{`export LC_ALL=en_US; date +'%B %-d, %Y'`}" >> abstracts.tex
	cat abstracts-top | grep -vE "documentclass|version|end.document" >> abstracts.tex
	echo '\newpage' >> abstracts.tex; \
	for i in *.tex; do \
	  if [ $$i != "abstracts.tex"  -a $$i != "abstracts-front.tex" -a $$i != "program.tex" ]; then \
	    cat $$i | sed '1,/begin{document}/d;/end{document}/d' \
	    | sed '/begin{otherlanguage/,/end{otherlanguage/d' \
	    | sed 's/bigskip/medskip/g' \
	    | sed 's/\\Large/\\Large\\bf/' \
	    | sed 's/\\large/\\large\\bf/' \
	    | sed 's/\\begin{thebibliography}/\\begin{spacing}{0.1}&/' \
	    | sed 's/\\end{thebibliography}/&\\end{spacing}/'>> abstracts.tex; \
	    echo "\vfill" >> abstracts.tex; \
	    echo "\newpage %$$i" >> abstracts.tex; \
	  fi; \
	done
	cat abstracts-bottom | grep -vE "documentclass|version|begin.document|end.document" >> abstracts.tex
	echo '\end{document}' >> abstracts.tex
	for i in `cat short.txt`; do \
	 sed -i "s/newpage.*%$$i.tex/bigskip/" abstracts.tex ; \
	done

abstracts-a5.pdf: abstracts.tex
	latex abstracts.tex
	latex abstracts.tex
	dvi2pdf.a5 abstracts abstracts-a5.pdf
	chmod a+r *.pdf

abstracts-2pps.pdf: abstracts-a5.pdf
	a52pps abstracts-a5.pdf abstracts-2pps.pdf
	chmod a+r *.pdf

abstracts-a4.pdf: abstracts-a5.pdf
	gs -q -sPAPERSIZE=a4 -dNOPAUSE -dBATCH -sDEVICE=pdfwrite -sOutputFile=abstracts-a4.pdf obl-abstr-logos.pdf abstracts-a5.pdf
	chmod a+r *.pdf


abstracts-a4-old.pdf: abstracts-a5.pdf
	a52a4 abstracts-a5.pdf abstracts-a4.pdf
	chmod a+r *.pdf

program:
	echo '\documentclass[10pt,a4paper]{article}' > program.tex
	echo '\def\version'"{`export LC_ALL=en_US; date +'%B %-d, %Y'`}" >> program.tex
	cat program-top | grep -vE "documentclass|version|end.document" >> program.tex
	cat program-frontpage-nofrills | grep -v "end.document" >> program.tex
	echo '\newpage' >> program.tex; \
	for i in *.tex; do \
	  if [ $$i != "abstracts.tex" -a $$i != "program.tex" ]; then \
	    echo $$i; cat $$i \
	    | sed '/begin{otherlanguage/,/end{otherlanguage/d' \
	    | sed '1,/begin{center}/d;/end{center}/,$$d' \
	    | sed 's/\\Large //;s/\\large //;s/\\sc //;s/\\bigskip//;s/\\medskip//' \
	    | sed '/small.it.*}$$/s/,[^}]*//' \
	    | grep -v '@' |grep -v "^ASDFASDF$$" >> program.tex; \
	    echo '\medskip' >> program.tex; \
	  fi; \
	done
	echo '\end{document}' >> program.tex

ASDF:
	    | sed '/begin{otherlanguage/,/end{otherlanguage/d' \
	    | sed '1,/begin{center}/d;/end{center}/,Gd' \

