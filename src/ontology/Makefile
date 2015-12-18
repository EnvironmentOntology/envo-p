OBO=http://purl.obolibrary.org/obo
USECAT= --catalog-xml catalog-v001.xml
SRC=envo-p.owl

MODS = entity_attribute_location entity_quality_location


# ----------------------------------------
# DESIGN PATTERNS AND TEMPLATES
# ----------------------------------------


all_modules: all_modules_owl all_modules_obo 
all_modules_owl: $(patsubst %, modules/%.owl, $(MODS))
all_modules_obo: $(patsubst %, modules/%.obo, $(MODS))

modules/%.owl: modules/%.tsv patterns/%.yaml curie_map.yaml
	apply-pattern.py -P curie_map.yaml -b http://purl.obolibrary.org/obo/ -i $< -p patterns/$*.yaml -G modules/$*-gci.owl > $@.tmp && mv $@.tmp $@

modules/%.obo: modules/%.owl
	owltools $< -o -f obo $@.tmp && grep -v ^owl-axioms $@.tmp > $@

# meta

modules/%.tsv:
	blip-findall -i patterns/spec.obo  -consult pmaker.pro -r envo -r chebi -r pato pattern_$*/3 -no_pred -label -use_tabs > $@
