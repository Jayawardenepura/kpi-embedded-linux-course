# based on https://linux.die.net/lkmpg/hello2.html by Peter Jay Salzman
# (!) using paths with spaces may not work with Kbuild

SRCDIR = src
BUILDDIR = .build
# this is what is used by Kbuild
obj-m += firstmod.o

# directory containing Makefile for kernel build
KBUILDDIR := /usr/lib/modules/$(shell uname -r)/build/

.PHONY: modules clean tidy

# recur to the original kernel Makefile with some additions
$(BUILDDIR):
	mkdir $@
	cp -r ./Makefile $(SRCDIR)/* $(BUILDDIR)/

.ONESHELL:
modules: $(BUILDDIR)
	cd $(BUILDDIR)
	$(MAKE) -C "$(KBUILDDIR)" M="$(PWD)/$(BUILDDIR)" modules

.ONESHELL:
tidy:
	cd $(BUILDDIR)
	$(MAKE) -C "$(KBUILDDIR)" M="$(PWD)/$(BUILDDIR)" clean

# we link original clean to target named tidy
.ONESHELL:
clean:
	cd $(BUILDDIR)
	-rm -f modules.order Module.symvers
	-rm -rf .tmp_versions
	-rm -f $(obj-m) $(obj-m:.o=.mod.o) $(obj-m:.o=.mod.c)
	-rm -f $(addsuffix .cmd,$(addprefix .,$(obj-m)))
	-rm -f $(addsuffix .cmd,$(addprefix .,$(obj-m:.o=.ko)))
	-rm -f $(addsuffix .cmd,$(addprefix .,$(obj-m:.o=.mod.o)))
