OBSPACKAGE=supportutils-plugin-susemanager
SVNDIRS=specs scripts man
VERSION=$(shell awk '/Version:/ { print $$2 }' specs/${OBSPACKAGE}.spec)
RELEASE=$(shell awk '/Release:/ { print $$2 }' specs/${OBSPACKAGE}.spec)
SRCDIR=$(OBSPACKAGE)-$(VERSION)
SRCFILE=$(SRCDIR).tar
BUILDDIR=/usr/src/packages

default: build

install: dist
	@echo ==================================================================
	@echo Installing source files into build directory
	@echo ==================================================================
	cp src/$(SRCFILE).gz $(BUILDDIR)/SOURCES
	cp specs/$(OBSPACKAGE).spec $(BUILDDIR)/SPECS
	@echo

uninstall:
	@echo ==================================================================
	@echo Uninstalling from build directory
	@echo ==================================================================
	rm -rf $(BUILDDIR)/SOURCES/$(SRCFILE).gz
	rm -rf $(BUILDDIR)/SPECS/$(OBSPACKAGE).spec
	rm -rf $(BUILDDIR)/BUILD/$(SRCDIR)
	rm -f $(BUILDDIR)/SRPMS/$(OBSPACKAGE)-$(VERSION)-$(RELEASE).src.rpm
	rm -f $(BUILDDIR)/RPMS/noarch/$(OBSPACKAGE)-$(VERSION)-$(RELEASE).noarch.rpm
	@echo

dist:
	@echo ==================================================================
	@echo Creating distribution source tarball
	@echo ==================================================================
	mkdir -p $(SRCDIR)
	for i in $(SVNDIRS); do cp $$i/* $(SRCDIR); done
	cp COPYING.GPLv2 $(SRCDIR)
	tar cf $(SRCFILE) $(SRCDIR)/*
	gzip -9f $(SRCFILE)
	rm -rf $(SRCDIR)
	mv -f $(SRCFILE).gz src
	@echo

clean:
	@echo ==================================================================
	@echo Cleaning up make files
	@echo ==================================================================
	rm -rf $(OBSPACKAGE)*
	rm -f src/$(SRCFILE).gz
	for i in $(SVNDIRS); do rm -f $$i/*~; done
	rm -f *~
	@echo

allclean: uninstall clean
	@echo
	@ls -al ${LS_OPTIONS}
	@echo

build: allclean install
	@echo ==================================================================
	@echo Building RPM package
	@echo ==================================================================
	rpmbuild -ba $(BUILDDIR)/SPECS/$(OBSPACKAGE).spec
	cp $(BUILDDIR)/SRPMS/$(OBSPACKAGE)-$(VERSION)-$(RELEASE).src.rpm .
	cp $(BUILDDIR)/RPMS/noarch/$(OBSPACKAGE)-$(VERSION)-$(RELEASE).noarch.rpm .
	@echo
	@ls -al ${LS_OPTIONS}
	@echo

help:
	@clear
	@make -v
	@echo
	@echo Make options for package: $(OBSPACKAGE)
	@echo make {install, uninstall, dist, clean, allclean, build[default]}
	@echo
