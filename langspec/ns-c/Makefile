empty :=
space := $(empty) $(empty)

CLASSPATH=$(subst  $(space),:,$(wildcard ../../lib/*.jar)):../../lib
SAXON=java -cp $(CLASSPATH) -jar ../../lib/saxon9he.jar
CALABASH=java -cp $(CLASSPATH) com.xmlcalabash.drivers.Main -c ../../.calabash

TARGETS=xproc-step.html ns-xproc-c-incl.xml

all: $(TARGETS)

xproc-step.html: ,xproc-step.xml
	@echo Format spec: $@
	@$(SAXON) -s:$< -xsl:../../style/dbspec.xsl -o:$@

,xproc-step.xml: xproc-step.xml ns-xproc-c-incl.xml \
               ../../style/docbook.xsl ../../style/dbspec.xsl \
               ../../style/xprocns.xsl ../../style/rngsyntax.xsl
	@echo Validate spec: $<
	@$(CALABASH) -isource=$< -oresult=$@ \
                    ../../style/validate.xpl schema=../../schema/dbspec.rng

ns-xproc-c-incl.xml: ../,langspec.xml ../../style/ns-extract.xsl
	@echo Extract names from $<
	@$(SAXON) -s:$< -xsl:../../style/ns-extract.xsl -o:$@ prefix=c:

clean:
	rm -f $(TARGETS) ,xproc-step.xml
