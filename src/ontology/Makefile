# ----------------------------------------
# Makefile for ecrro
# Generated using ontology-starter-kit
# ----------------------------------------
# <do not edit above this line>

# 5/2/2019 R. Duerr - removed all imports except RO and bfo (line 62, 269-326)

# ----------------------------------------
# Standard Constants
# ----------------------------------------
# these can be overwritten on the command line

URIBASE=                    http://purl.obolibrary.org/obo
ONT=                        ecrro
ONTBASE=                    $(URIBASE)/$(ONT)
EDIT_FORMAT=                owl
SRC =                       $(ONT)-edit.$(EDIT_FORMAT)
ROBOT=                      robot
RELEASEDIR=                 ../..
PATTERNDIR=                 ../patterns
REPORTDIR=                 reports
SPARQLDIR =                 ../sparql
REPORT_FAIL_ON =            ERROR
OBO_FORMAT_OPTIONS =        
SPARQL_VALIDATION_CHECKS =  equivalent-classes trailing-whitespace owldef-self-reference xref-syntax nolabels
SPARQL_EXPORTS =            basic-report class-count-by-prefix edges xrefs obsoletes synonyms

TODAY   := $(shell date +%Y-%m-%d)


FORMATS = $(sort  owl obo json owl)
FORMATS_INCL_TSV = $(sort $(FORMAT) tsv)
RELEASE_ARTEFACTS = $(sort  full base base full)
 
# ----------------------------------------
# Top-level targets
# ----------------------------------------

.PHONY: .FORCE
all: all_imports all_main all_subsets sparql_test all_reports all_assets
test: sparql_test all_reports
	$(ROBOT) reason --input $(SRC) --reasoner ELK --output test.owl && rm test.owl && echo "Success"

## -- main targets --
##
## By default this is the cross-product of {ont, ont-base} x FORMATS

MAIN_PRODUCTS = $(sort $(foreach r,$(RELEASE_ARTEFACTS), $(ONT)-$(r)) $(ONT))
MAIN_GZIPPED = 
MAIN_FILES = $(foreach n,$(MAIN_PRODUCTS), $(foreach f,$(FORMATS), $(n).$(f))) $(MAIN_GZIPPED)

all_main: $(MAIN_FILES)

## -- import targets --
##
## By default this is the cross-product of IMPORT_MODULES x FORMATS


## IMPORTS =  ro bfo swo obi ncit dcat prov-o sio

IMPORTS =  ro bfo

IMPORT_ROOTS = $(patsubst %, imports/%_import, $(IMPORTS))
IMPORT_FILES = $(foreach n,$(IMPORT_ROOTS), $(foreach f,$(FORMATS), $(n).$(f)))
IMPORT_OWL_FILES = $(foreach n,$(IMPORT_ROOTS), $(n).owl)

all_imports: $(IMPORT_FILES)
 
all_imports_owl: $(foreach n,$(IMPORT_ROOTS), $(n).owl)
 
all_imports_obo: $(foreach n,$(IMPORT_ROOTS), $(n).obo)
 
all_imports_json: $(foreach n,$(IMPORT_ROOTS), $(n).json)


## -- subset targets --
##
## By default this is the cross-product of SUBSETS x FORMATS
## Note we also include TSV as a format


SUBSETS = 

SUBSET_ROOTS = $(patsubst %, subsets/%, $(SUBSETS))
SUBSET_FILES = $(foreach n,$(SUBSET_ROOTS), $(foreach f,$(FORMATS_INCL_TSV), $(n).$(f)))

all_subsets: $(SUBSET_FILES)

## -- dosdp pattern targets --
##


PATTERNS = 

PATTERN_ROOTS = $(patsubst %, $(PATTERNDIR)/%, $(PATTERNS))
PATTERN_FILES = $(foreach n,$(PATTERN_ROOTS), $(n).owl)

all_patterns: $(PATTERN_FILES)

## -- dosdp pattern targets --
##

OBO_REPORT = $(ONT)-edit-obo-report
REPORTS = $(OBO_REPORT)
REPORT_FILES = $(patsubst %, $(REPORTDIR)/%.tsv, $(REPORTS))

all_reports: all_reports_onestep $(REPORT_FILES)

## -- all files/assets --

ASSETS = \
  $(IMPORT_FILES) \
  $(MAIN_FILES) \
  $(REPORT_FILES) \
  $(SUBSET_FILES)

all_assets: $(ASSETS)

show_assets:
	echo $(ASSETS)
	du -sh $(ASSETS)


# ----------------------------------------
# Release Management
# ----------------------------------------

KEEPRELATIONS=keeprelations.txt
ONTOLOGYTERMS=ontologyterms.txt

# This should be executed by the release manager whenever time comes to make a release.
# It will ensure that all assets/files are fresh, and will copy to release folder
prepare_release: $(ASSETS) $(PATTERN_RELEASE_FILES)
	rsync -R $(ASSETS) $(RELEASEDIR) &&\
  echo "Release files are now in $(RELEASEDIR) - now you should commit, push and make a release on github"

prepare_initial_release: prepare_release
	cd $(RELEASEDIR) && git add $(ASSETS)

# ----------------------------------------
# Export formats
# ----------------------------------------


$(ONT)-full.obo: $(ONT)-full.owl
	$(ROBOT) annotate --input $< --ontology-iri $(ONTBASE)/$@ --version-iri $(ONTBASE)/releases/$(TODAY)/$@ \
		convert --check false -f obo $(OBO_FORMAT_OPTIONS) -o $@.tmp.obo && grep -v ^owl-axioms $@.tmp.obo > $@ && rm $@.tmp.obo
$(ONT)-full.json: $(ONT)-full.owl
	$(ROBOT) annotate --input $< --ontology-iri $(ONTBASE)/$@ --version-iri $(ONTBASE)/releases/$(TODAY)/$@ \
		convert --check false -f json -o $@.tmp.json && mv $@.tmp.json $@
$(ONT)-base.obo: $(ONT)-base.owl
	$(ROBOT) annotate --input $< --ontology-iri $(ONTBASE)/$@ --version-iri $(ONTBASE)/releases/$(TODAY)/$@ \
		convert --check false -f obo $(OBO_FORMAT_OPTIONS) -o $@.tmp.obo && grep -v ^owl-axioms $@.tmp.obo > $@ && rm $@.tmp.obo
$(ONT)-base.json: $(ONT)-base.owl
	$(ROBOT) annotate --input $< --ontology-iri $(ONTBASE)/$@ --version-iri $(ONTBASE)/releases/$(TODAY)/$@ \
		convert --check false -f json -o $@.tmp.json && mv $@.tmp.json $@
# Main release artefacts
$(ONT).owl: $(ONT)-full.owl
	$(ROBOT) annotate --input $< --ontology-iri $(URIBASE)/$@ --version-iri $(ONTBASE)/releases/$(TODAY) \
		convert -o $@.tmp.owl && mv $@.tmp.owl $@

$(ONT).obo: $(ONT)-full.owl
	$(ROBOT) annotate --input $< --ontology-iri $(URIBASE)/$@ --version-iri $(ONTBASE)/releases/$(TODAY) \
		convert --check false -f obo $(OBO_FORMAT_OPTIONS) -o $@.tmp.obo && grep -v ^owl-axioms $@.tmp.obo > $@ && rm $@.tmp.obo
$(ONT).json: $(ONT)-full.owl
	$(ROBOT) annotate --input $< --ontology-iri $(URIBASE)/$@ --version-iri $(ONTBASE)/releases/$(TODAY) \
		convert --check false -f json -o $@.tmp.json && mv $@.tmp.json $@
# ----------------------------------------
# Initiating Step: Reason over source
# ----------------------------------------

ANNOTATE_VERSION_IRI = annotate -V $(ONTBASE)/releases/`date +%Y-%m-%d`/$@.owl

# by default we use ELK to perform a reason-relax-reduce chain
# after that we annotate the ontology with the release versionInfo


$(ONTOLOGYTERMS): $(SRC) all_imports
	$(ROBOT) query --use-graphs true -f csv -i $< --query ../sparql/ecrro_terms.sparql $@

 

 

 





# base: OTHER sources of interest, such as definitions owl
$(ONT)-base.owl: $(SRC) $(OTHER_SRC)
	$(ROBOT) remove --input $< --select imports \
		annotate --ontology-iri $(ONTBASE)/$@ --version-iri $(ONTBASE)/releases/$(TODAY)/$@ --output $@.tmp.owl && mv $@.tmp.owl $@

# Full: The full artefacts with imports merged, reasoned
$(ONT)-full.owl: $(SRC) $(OTHER_SRC)
	$(ROBOT) merge --input $< \
		reason --reasoner ELK \
		relax \
		reduce -r ELK \
		annotate --ontology-iri $(ONTBASE)/$@ --version-iri $(ONTBASE)/releases/$(TODAY)/$@ --output $@.tmp.owl && mv $@.tmp.owl $@

# ----------------------------------------
# Import modules
# ----------------------------------------
# Most ontologies are modularly constructed using portions of other ontologies
# These live in the imports/ folder

# seed.txt contains all referenced entities


seed.txt: $(SRC)
	@if [ $(IMP) = true ]; then $(ROBOT) query --use-graphs true -f csv -i $< --query ../sparql/terms.sparql $@; fi


# Generate terms.txt for each import.  (Assume OBO-style Possibly hacky step?)
# Should be able to drop this if robot can just take a big messy list of terms as input.

imports/%_terms_combined.txt: seed.txt imports/%_terms.txt
	@if [ $(IMP) = true ]; then cat $^ | grep -v ^# | sort | uniq >  $@; fi

# -- Generate Import Modules --
#
# This pattern uses ROBOT to generate an import module
imports/%_import.owl: mirror/%.owl imports/%_terms_combined.txt
	@if [ $(IMP) = true ]; then $(ROBOT) extract -i $< -T imports/$*_terms_combined.txt --method BOT \
		annotate --ontology-iri $(ONTBASE)/$@ --version-iri $(ONTBASE)/releases/$(TODAY)/$@ --output $@.tmp.owl && mv $@.tmp.owl $@; fi
.PRECIOUS: imports/%_import.owl

# convert imports to obo.
# this can be useful for spot-checks and diffs.
# we set strict mode to false by default. For discussion see https://github.com/owlcs/owlapi/issues/752
imports/%_import.obo: imports/%_import.owl
	@if [ $(IMP) = true ]; then $(ROBOT) convert --check false -i $< -f obo -o $@.tmp.obo && mv $@.tmp.obo $@; fi
imports/%_import.json: imports/%_import.owl
	@if [ $(IMP) = true ]; then $(ROBOT) convert --check false -i $< -f json -o $@.tmp.json && mv $@.tmp.json $@; fi
# ----------------------------------------
# Mirroring upstream ontologies
# ----------------------------------------
#

IMP=true # Global parameter to bypass import generation
MIR=true # Global parameter to bypass mirror generation



## ONTOLOGY: ro
## Copy of ro is re-downloaded whenever source changes
mirror/ro.trigger: $(SRC)

mirror/ro.owl: mirror/ro.trigger
	@if [ $(MIR) = true ] && [ $(IMP) = true ]; then $(ROBOT) convert -I http://purl.obolibrary.org/obo/ro/core.owl -o $@.tmp.owl && mv $@.tmp.owl $@; fi

.PRECIOUS: mirror/%.owl


## ONTOLOGY: bfo
## Copy of bfo is re-downloaded whenever source changes
mirror/bfo.trigger: $(SRC)

mirror/bfo.owl: mirror/bfo.trigger
	@if [ $(MIR) = true ] && [ $(IMP) = true ]; then $(ROBOT) convert -I $(URIBASE)/bfo.owl -o $@.tmp.owl && mv $@.tmp.owl $@; fi

.PRECIOUS: mirror/%.owl


## ONTOLOGY: swo
## Copy of swo is re-downloaded whenever source changes
## mirror/swo.trigger: $(SRC)

## mirror/swo.owl: mirror/swo.trigger
##	@if [ $(MIR) = true ] && [ $(IMP) = true ]; then $(ROBOT) convert -I $(URIBASE)/swo.owl -o $@.tmp.owl && mv $@.tmp.owl $@; fi

## .PRECIOUS: mirror/%.owl


## ONTOLOGY: obi
## Copy of obi is re-downloaded whenever source changes
## mirror/obi.trigger: $(SRC)

## mirror/obi.owl: mirror/obi.trigger
## 	@if [ $(MIR) = true ] && [ $(IMP) = true ]; then $(ROBOT) convert -I $(URIBASE)/obi.owl -o $@.tmp.owl && mv $@.tmp.owl $@; fi

## .PRECIOUS: mirror/%.owl


## ONTOLOGY: ncit
## Copy of ncit is re-downloaded whenever source changes
## mirror/ncit.trigger: $(SRC)

## mirror/ncit.owl: mirror/ncit.trigger
## 	@if [ $(MIR) = true ] && [ $(IMP) = true ]; then $(ROBOT) convert -I $(URIBASE)/ncit.owl -o $@.tmp.owl && mv $@.tmp.owl $@; fi

## .PRECIOUS: mirror/%.owl


## ONTOLOGY: dcat
## Copy of dcat is re-downloaded whenever source changes
## mirror/dcat.trigger: $(SRC)

## mirror/dcat.owl: mirror/dcat.trigger
## 	@if [ $(MIR) = true ] && [ $(IMP) = true ]; then $(ROBOT) convert -I https://www.w3.org/ns/dcat.rdf -o $@.tmp.owl && mv $@.tmp.owl $@; fi

## .PRECIOUS: mirror/%.owl


## ONTOLOGY: prov-o
## Copy of prov-o is re-downloaded whenever source changes
## mirror/prov-o.trigger: $(SRC)

## mirror/prov-o.owl: mirror/prov-o.trigger
## 	@if [ $(MIR) = true ] && [ $(IMP) = true ]; then $(ROBOT) convert -I http://www.w3.org/ns/prov-o -o $@.tmp.owl && mv $@.tmp.owl $@; fi

## .PRECIOUS: mirror/%.owl


## ONTOLOGY: sio
## Copy of sio is re-downloaded whenever source changes
## mirror/sio.trigger: $(SRC)

## mirror/sio.owl: mirror/sio.trigger
## 	@if [ $(MIR) = true ] && [ $(IMP) = true ]; then $(ROBOT) convert -I https://raw.githubusercontent.com/micheldumontier/semanticscience/master/ontology/sio/release/sio-release.owl -o $@.tmp.owl && mv $@.tmp.owl $@; fi

## .PRECIOUS: mirror/%.owl



# ----------------------------------------
# Subsets
# ----------------------------------------
subsets/%.tsv: subsets/%.owl
	$(ROBOT) query -f tsv -i $< -s ../sparql/labels.sparql $@
subsets/%.owl: $(ONT).owl
	owltools --use-catalog $< --extract-ontology-subset --fill-gaps --subset $* -o $@.tmp.owl && mv $@.tmp.owl $@


# ----------------------------------------
# Release
# ----------------------------------------
# copy from staging area (this directory) to top-level
release: $(ONT).owl $(ONT).obo
	cp $^ $(RELEASEDIR) && cp imports/* $(RELEASEDIR)/imports

# ----------------------------------------
# Sparql queries: Q/C
# ----------------------------------------

# these live in the ../sparql directory, and have suffix -violation.sparql
# adding the name here will make the violation check live.
# NOTE: these will soon be phased out and replaced by robot-report

#  run all violation checks
SPARQL_VALIDATION_QUERIES = $(foreach V,$(SPARQL_VALIDATION_CHECKS),$(SPARQLDIR)/$V-violation.sparql)
sparql_test: $(SRC)
	$(ROBOT) verify  --catalog catalog-v001.xml -i $< --queries $(SPARQL_VALIDATION_QUERIES) -O reports/

# ----------------------------------------
# ROBOT report
# ----------------------------------------
reports/%-obo-report.tsv: %.owl
	$(ROBOT) report -i $< --fail-on $(REPORT_FAIL_ON) -o $@

# ----------------------------------------
# Sparql queries: Exports
# ----------------------------------------

SPARQL_EXPORTS_ARGS = $(foreach V,$(SPARQL_EXPORTS),-s $(SPARQLDIR)/$V.sparql reports/$V.tsv)
# This combines all into one single command
all_reports_onestep: $(SRC)
	$(ROBOT) query -f tsv -i $< $(SPARQL_EXPORTS_ARGS)


# ----------------------------------------
# Docker (experimental)
# ----------------------------------------
IM=build-$(ONT)
build-docker:
	docker build -t $(ONT) .



include ecrro.Makefile





