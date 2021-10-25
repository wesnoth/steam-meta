DOMAIN := wesnoth-steam-meta

PODIR := po
STAMPDIR := .stamp

CONF := languages.conf

LANGUAGES := $(basename $(notdir $(wildcard $(PODIR)/*.po)))

POT := $(PODIR)/$(DOMAIN).pot
PO := $(addprefix $(PODIR)/, $(LANGUAGES:=.po))

TEMPLATE  := app_metadata.txt
OUTPUT    := app_metadata.%.txt
OUTPUT_SH := app_metadata.*.txt

OUTPUTS := $(foreach lang, $(LANGUAGES), $(shell echo $(TEMPLATE) | sed -r 's/\.txt$$/.$(lang).txt/'))
OUTPUT_STAMP := $(addprefix $(STAMPDIR)/, $(foreach lang, $(LANGUAGES), $(shell echo $(TEMPLATE) | sed -r 's/\.txt$$/.$(lang).txt/')))

PO4A_OPTIONS         = -f text -M utf-8 -o neverwrap
PO4A_CATALOGUE_META  = --copyright-holder 'Wesnoth, Inc.' --package-name '$(DOMAIN)' --package-version 1.0.0
# Do not change. This is deliberate.
PO4A_THRESHOLD       = 80

PO4A_UPDATEPO       := po4a-updatepo $(PO4A_OPTIONS) $(PO4A_CATALOGUE_META) -m $(TEMPLATE)
PO4A_GETTEXTIZE     := po4a-gettextize $(PO4A_OPTIONS) $(PO4A_CATALOGUE_META) -L utf-8 -m $(TEMPLATE)
PO4A_TRANSLATE      := po4a-translate $(PO4A_OPTIONS) -L utf-8 -m $(TEMPLATE) -k $(PO4A_THRESHOLD)


.PHONY: all bootstrap update-pot update-po update-txt clean distclean

all: update-pot update-po update-txt

bootstrap: update-pot
	@for f in `sed -rn '/^\s*[^#]/ { s/^([^:]+):.*$$/\1/; p }' $(CONF)`; do \
		echo "  PO          $$f"; \
		$(PO4A_UPDATEPO) -p $(PODIR)/$$f.po 2> /dev/null; \
	done

update-pot: $(POT)

update-po: $(PO)

update-txt: $(OUTPUT_STAMP)

stats:
	@cd $(PODIR) && \
	for i in *.po; do \
		echo -n "$$i: "; \
		msgfmt --statistics -o /dev/null $$i; \
	done

clean:
	rm -rf $(PODIR)/*~ $(STAMPDIR)

distclean: clean
	rm -rf $(OUTPUT_SH)


$(POT): $(TEMPLATE)
	@echo "  POT         $@" && \
	$(PO4A_GETTEXTIZE) -p $(POT)

$(PO): $(TEMPLATE)
	@echo "  PO          $@" && \
	dos2unix -q $@ && \
	$(PO4A_UPDATEPO) -p $@ && \
	touch $@

$(STAMPDIR)/$(OUTPUT): $(TEMPLATE) $(PODIR)/%.po
	@out=`basename $@` && \
	template=`echo $$out | sed -r 's/^(.*)\.(.*)\.txt$$/\1.txt/'` && \
	lang=`echo $$out | sed -r 's/^.*\.(.*)\.txt$$/\1/'` && \
	echo "  PO4A        $$out" && \
	$(PO4A_TRANSLATE) -p $(PODIR)/$$lang.po -l $$out
	@mkdir -p $(STAMPDIR) && touch $@
