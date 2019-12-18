#
# Makefile to compile Linux Kernel with given patches
#

FOLDER=linux
URL=git://git.phytec.de/linux-imx
TAG=v4.1.15-phy3
# Patches skipped - e.g. because they are in the current revision not needed anymore
SKIPPED_PATCHES=""

all: compile

$(FOLDER):
ifeq (x$(wildcard $(FOLDER))z,xz)
	git clone --depth=1 --branch $(TAG) $(URL) $(FOLDER)
	cd $(FOLDER) && for PATCH in $(wildcard *.patch) ; do \
		if ! echo " $(SKIPPED_PATCHES) " | grep -q " $$PATCH " ; then \
			echo " >> Applying $$PATCH..." ; \
			patch --force -p1 < ../$$PATCH || exit 1 ; \
		else \
			echo " >> Skipping patch $$PATCH..." ; \
		fi ; \
	done
else
	@echo " >> Folder '$(FOLDER)' already created."
endif

patch: | $(FOLDER)
	@echo " >> Patched tag $(TAG) successfully!"

compile: | $(FOLDER)
	cd $(FOLDER) && $(MAKE) defconfig
	cd $(FOLDER) && $(MAKE) -j9

clean:
	rm -rf $(FOLDER)

.PHONY: all patch compile clean
