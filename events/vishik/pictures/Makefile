title="`pwd| sed 's/.*\///'`"


all: lower thumbnails index permissions

permissions:
	chown comech.comech *
	chmod a+r *

1024:
	mkdir 1024
	for i in img*jpg; do \
	   convert -size 1024x1024 $$i -resize 1024x1024 1024/a$$i; done

clean:

remove-no-r:
	-for i in img_*r.jpg; do \
	  orig="`echo $$i |sed 's/r\.jpg/.jpg/'`"; \
	  if [ -f $$orig ]; then rm $$orig thumb_$$orig; fi; \
	done

mod:
	chmod ug+rw *
	chmod a+r *

index:
	-rm index.html
	echo "<html><head>" > index.html
	echo "<title>Pictures from Mark Vishik conference</title>" >> index.html
	 #echo "<base href=\"http://www.math.tamu.edu/~comech/$$(pwd|sed 's/^.*\/pub\///')/\"> " >> index.html
	echo "</head>" >> index.html
	echo "<body>" >> index.html
	echo "<h1 align=center>Pictures from Mark Vishik conference</h1>" >> index.html
	-for i in *; do if [ -d $$i ]; then \
	  echo "<a href=\"$$i/index.html\">$$i</a><br>" >> index.html; \
	fi; done
	-for th in $$(ls thumb_*); do \
	  img=$$(echo $$th|sed 's/thumb_//'); \
	  imgr=$$(echo $$img|sed 's/\.jpg$$/r.jpg/'); \
	  if [ -r $$img ]; then \
	    if [ ! -r $$imgr ]; then \
	      echo -n "  <a href=\"$$img\">" >> index.html; \
	      echo -n "<img src=\"$$th\""  >> index.html; \
              echo " border=\"0\" hspace=4 vspace=4></a>" >> index.html; \
	      echo "" >> index.html ; \
	    fi; \
	  else img=`echo $$img|sed 's/\.jpg/.avi/'`; \
	    if [ -r $$img ]; then \
	      echo -n "  <a href=\"$$img\">" >> index.html; \
	      echo -n "<img src=\"$$th\"" >> index.html; \
	      echo " border=\"4\" hspace=0 vspace=0></a>" >> index.html; \
	      echo "" >> index.html ; \
	    else \
	      echo " * * * TARGET FILE MISSING FOR $$th * * * "; \
	      ls -l $$img; \
	      rm $$th; \
	    fi; \
	  fi; \
	done 
	echo "</body></html>" >> index.html
	ln -sf index.html 00index.html
	chmod a+r index.html
	-chmod a+r *.jpg *.avi

lower:
	-for i in *.JPG *.jpg *.AVI *.avi; do \
	mv $$i `echo $$i|tr 'A-Z' 'a-z'` 2>&1 | grep -v "the.same.file" ; done

thumbnails:
	-for i in *.jpg; do \
	  if [ -r $$i -a ! -r "`echo $$i | sed 's/\.jpg/r.jpg/'`" -a "`echo $$i | grep '^thumb_'`" = "" ]; then \
	    [ -r thumb_$$i ] || convert -size 200x100 $$i -resize 200x100 thumb_$$i; \
	  fi; \
	done
	-for i in *.avi; do \
	  if [ -r $$i -a "`echo $$i | grep '^thumb_'`" = "" ]; then \
	    th=thumb_`echo $$i|sed 's/\.avi/.jpg/'`; \
	    [ -r $$th ] ||convert -size 200x100 $$i -resize 200x100 +profile '*' $$th; \
	  fi; \
	done

get-images: 
	mv gphoto2.files gphoto2.files.old 
	gphoto2 --list-files > gphoto2.files

clean:
	for i in thumb_*r.jpg; do rm -f "`echo $$i|sed 's/r\.jpg/.jpg/'`"; done
