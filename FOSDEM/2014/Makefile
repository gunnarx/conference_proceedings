default: all

Makefile.timestamp: makedirs

Makefile:: Makefile.timestamp
	touch $@

all: makedirs dirs

makedirs: index.html
	for i in `grep 'td><a href=".[^"]*/"' $< | cut -f 2 -d '"'`; do \
	  [ -e $$i ] || mkdir $$i; \
	  [ -h $$i/Makefile.generated ] || \
            [ $$i == ../ ] || \
            ln -s ../Makefile.subdir $$i/Makefile.generated && \
	    touch Makefile.timestamp; \
	done

PHONY = 

define DIR_template
$(1):
	make -f Makefile.generated -C $(1)

PHONY += $(1)
endef

DIRS = $(patsubst %/Makefile.generated,%,$(wildcard */Makefile.generated))
$(info $(DIRS))

$(foreach dir,$(DIRS),$(eval $(call DIR_template, $(dir))))

dirs: $(DIRS)

BASE = https://video.fosdem.org/2014/

index.html:
	wget -O $@.new $(BASE)
	mv $@.new $@

.PHONY: default all index.html dirs $(PHONY)
