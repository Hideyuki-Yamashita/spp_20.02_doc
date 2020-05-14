# SPDX-License-Identifier: BSD-3-Clause
# Copyright(c) 2018 Nippon Telegraph and Telephone Corporation

VERSION := 19.11

ifneq ($(RTE_SDK),)
# Default target, can be overriden by command line or environment
RTE_TARGET ?= x86_64-native-linux-gcc

include $(RTE_SDK)/mk/rte.vars.mk

DEPDIRS-tools := src

DIRS-y += src
DIRS-y += tools

include $(RTE_SDK)/mk/rte.extsubdir.mk
endif

.PHONY: showversion
showversion:
	@echo $(VERSION)

# Compile RST documents
DOC_ROOT = docs/guides

# Clean all files generated while compilation. It consists of two
# tasks, _dist-clean and dist-clean. First one is for removing the
# generated files, and second one is just for removing `_postclean`
# which is generated after the first task.
.PHONY: dist-clean
dist-clean: _dist-clean
	rm -f $(wildcard src/drivers/*/_postclean)

.PHONY: _dist-clean
_dist-clean:
	make clean
	rm -rf $(wildcard src/*/$(RTE_TARGET))
	rm -rf $(wildcard src/*/shared)
	rm -rf $(wildcard src/drivers/*/$(RTE_TARGET))
	rm -f $(wildcard src/*/*.pyc)
	rm -f $(wildcard src/*/*/*.pyc)
	rm -rf $(wildcard src/*/__pycache__)
	rm -rf $(wildcard src/*/*/__pycache__)
	rm -rf $(wildcard tools/vdev_test/build)
	rm -rf $(wildcard tools/vdev_test/$(RTE_TARGET))

.PHONY: doc
doc: doc-all
doc-all: doc-pdf doc-html

.PHONY: doc-html
doc-html:
	make -C $(DOC_ROOT) html

RTE_PDF_DPI := 300
IMG_DIR := docs/guides/images
SVGS := $(wildcard $(IMG_DIR)/*.svg)
SVGS += $(wildcard $(IMG_DIR)/*/*.svg)
SVGS += $(wildcard $(IMG_DIR)/*/*/*.svg)
PDFS := $(SVGS:%.svg=%.pdf)

%.pdf: %.svg
	inkscape -d $(RTE_PDF_DPI) -D -f $< -A $@ $(RTE_INKSCAPE_VERBOSE)

.PHONY: doc-pdf
doc-pdf: $(PDFS)
	make -C $(DOC_ROOT) latexpdf
	@echo "Succeeded to generate '$(DOC_ROOT)/_build/latex/SoftPatchPanel.pdf'"

.PHONY: doc-clean
doc-clean:
	find $(DOC_ROOT)/images/ -type f -name "*.pdf" -delete
	make -C $(DOC_ROOT) clean
