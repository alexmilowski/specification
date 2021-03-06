empty :=
space := $(empty) $(empty)

CLASSPATH=$(subst  $(space),:,$(wildcard ../lib/*.jar)):../lib
SAXON=java -cp $(CLASSPATH) -jar ../lib/saxon9he.jar
CALABASH=java -cp $(CLASSPATH) com.xmlcalabash.drivers.Main -c ../.calabash

TARGETS=langspec.html changelog.html diff.html error-list.xml \
        pipeline-library.xml typed-pipeline-library.xml

all: $(TARGETS)
	$(MAKE) -C ns-p
	$(MAKE) -C ns-c
	$(MAKE) -C ns-err

diff.html: langspec.html
	../lib/make-diff.sh

langspec.html: ,langspec.xml typed-pipeline-library.xml \
               ../style/docbook.xsl ../style/dbspec.xsl \
               ../style/xprocns.xsl ../style/rngsyntax.xsl
	@echo Format spec: $@
	@$(CALABASH) -isource=$< -oresult=$@ \
	            -pschemaext.schema=../../../../../schema/dbspec.rng \
                    ../style/formatspec.xpl

changelog.html: changelog.xml
	@echo Format changelog
	@$(CALABASH) -isource=$< -oresult=$@ ../style/formatspec.xpl

,langspec.xml: langspec.xml conformance.xml error-codes.xml glossary.xml \
               language-summary.xml mediatype.xml namespace-fixup.xml \
               parallel.xml references.xml \
               serialization-options-for-escape-markup.xml \
               serialization-options.xml serialization-options.xml \
               serialization-options.xml standard-components.xml
	$(MAKE) -C examples
	@echo Validate spec: $<
	@$(CALABASH) -isource=$< -oresult=$@ \
                    ../style/validate.xpl \
                    schema=../schema/dbspec.rng \
                    schematron=../schema/docbook.sch

error-list.xml: ,langspec.xml
	@echo Extract error list: $@
	@$(SAXON) -s:$< -xsl:../style/error-list.xsl -o:$@

pipeline-library.xml: ,langspec.xml
	@echo Extract pipeline library: $@
	@$(SAXON) -s:$< -xsl:../style/pipeline-library.xsl -o:$@

typed-pipeline-library.xml: ,langspec.xml
	@echo Extract typed pipeline library: $@
	@$(SAXON) -s:$< -xsl:../style/typed-pipeline-library.xsl -o:$@
	@$(MAKE) -C schemas

clean:
	rm -f $(TARGETS) ,langspec.xml
	$(MAKE) -C schemas clean
	$(MAKE) -C examples clean
	$(MAKE) -C ns-c clean
	$(MAKE) -C ns-err clean
	$(MAKE) -C ns-p clean
