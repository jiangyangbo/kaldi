# SHELL += -x

CXX = g++

# On Mac OS 10.9, g++ is actually clang in disguise which by default uses the
# new c++ standard library libc++. Since openfst uses stuff from the tr1
# namespace, we need to tell clang to use libstdc++ instead.
COMPILER = $(shell $(CXX) -v 2>&1 )
ifeq ($(findstring clang,$(COMPILER)),clang)
  CXXFLAGS += -stdlib=libstdc++
  LDFLAGS += -stdlib=libstdc++
endif

all: check_required_programs sph2pipe atlas irstlm_tgt sclite openfst 

check_required_programs:
	which aclocal
	which automake
	which autoconf
	which libtoolize || which glibtoolize
	which svn
	which gcc
	which $(CXX)
	which patch
	which tar

clean: openfst_cleaned sclite_cleaned  irstlm_cleaned

openfst_cleaned:
	$(MAKE) -C openfst-1.2.10 clean

irstlm_cleaned:
	$(MAKE) -C irstlm clean 

sclite_cleaned:
	$(MAKE) -C sctk-2.4.0 clean

distclean:
	rm -rf openfst-1.2.10/
	rm -rf sctk-2.4.0/
	rm -rf ATLAS/
	rm -rf irstlm/
	rm -rf sph2pipe_v2.5/
	rm -rf sph2pipe_v2.5.tar.gz
	rm -rf atlas3.8.3.tar.gz
	rm -rf sctk-2.4.0-20091110-0958.tar.bz2
	rm -rf openfst-1.2.10.tar.gz
	rm -f openfst


.PHONY: openfst # so target will be made even though "openfst" exists.	
openfst: openfst_compiled openfst-1.2.10/lib

.PHONY: openfst_compiled
openfst_compiled: openfst-1.2.10/Makefile
	cd openfst-1.2.10/ && \
	$(MAKE) install

openfst-1.2.10/lib: | openfst-1.2.10/Makefile
	-cd openfst-1.2.10 && [ -d lib64 ] && [ ! -d lib ] && ln -s lib64 lib

openfst-1.2.10/Makefile: openfst-1.2.10/.patched
	cd openfst-1.2.10/; \
	./configure --prefix=`pwd` --enable-static --enable-shared --enable-far --enable-ngram-fsts CXX=$(CXX) CXXFLAGS="$(CXXFLAGS)" LDFLAGS="$(LDFLAGS)" LIBS="-ldl"

openfst-1.2.10/.patched: | openfst-1.2.10
	-cd openfst-1.2.10/src/include/fst; \
	patch -p0 -N < ../../../../openfst.patch;
	touch .patched 	

openfst-1.2.10: openfst-1.2.10.tar.gz
	tar xozf openfst-1.2.10.tar.gz
	-rm openfst
	-ln -s openfst-1.2.10 openfst 

openfst-1.2.10.tar.gz: 
	wget -T 10 -t 3 --no-check-certificate https://sourceforge.net/projects/kaldi/files/openfst-1.2.10.tar.gz

sclite: sclite_compiled

.PHONY: sclite_compiled
sclite_compiled: sctk_patched
	cd sctk-2.4.0; \
	$(MAKE) config && $(MAKE) all && $(MAKE) install && $(MAKE) doc

sctk_patched: | sctk-2.4.0
	cd sctk-2.4.0/; \
	for x in src/asclite/core/recording.{h,cpp}; do \
	  sed 's/Filter::Filter/::Filter/' $$x > tmpf; mv tmpf $$x; \
	done;
	touch $@ 
 
sctk-2.4.0: sctk-2.4.0-20091110-0958.tar.bz2
	tar xojf sctk-2.4.0-20091110-0958.tar.bz2

sctk-2.4.0-20091110-0958.tar.bz2:
	wget --no-check-certificate -T 10 -t 3 https://sourceforge.net/projects/kaldi/files/sctk-2.4.0-20091110-0958.tar.bz2

irstlm_tgt: irstlm_compiled

.PHONY: irstlm_compiled
irstlm_compiled: irstlm/Makefile
	cd irstlm/; \
	$(MAKE); $(MAKE) install

irstlm/Makefile: irstlm/.patched
	cd irstlm; \
	automake --version | grep 1.13.1 >/dev/null && \
	sed s:AM_CONFIG_HEADER:AC_CONFIG_HEADERS: <configure.in >configure.ac; \
	(./regenerate-makefiles.sh || ./regenerate-makefiles.sh) && \
	./configure --prefix `pwd` 


irstlm/.patched: | irstlm
	-cd irstlm;\
	patch --verbose -N -p0 < ../interpolatedwrite-5.60.02.patch
	patch --verbose -N -p0 < ../irstlm.patch; \
	touch .patched

irstlm:
	svn -r 398 co --non-interactive --trust-server-cert https://svn.code.sf.net/p/irstlm/code/trunk irstlm

atlas: ATLAS/include/cblas.h

ATLAS/include/cblas.h: | atlas3.8.3.tar.gz
	tar xozf atlas3.8.3.tar.gz ATLAS/include

atlas3.8.3.tar.gz:
	wget -T 10 -t 3 http://sourceforge.net/projects/math-atlas/files/Stable/3.8.3/atlas3.8.3.tar.gz

sph2pipe: sph2pipe_compiled

sph2pipe_compiled: sph2pipe_v2.5/sph2pipe

sph2pipe_v2.5/sph2pipe: | sph2pipe_v2.5
	cd sph2pipe_v2.5/; \
	$(CC) -o sph2pipe  *.c -lm

sph2pipe_v2.5: sph2pipe_v2.5.tar.gz
	tar xzf sph2pipe_v2.5.tar.gz

sph2pipe_v2.5.tar.gz:
	wget --no-check-certificate -T 10  https://sourceforge.net/projects/kaldi/files/sph2pipe_v2.5.tar.gz || \
	wget -T 10 -t 3 http://www.openslr.org/resources/3/sph2pipe_v2.5.tar.gz

