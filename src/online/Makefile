all:

EXTRA_CXXFLAGS = -Wno-sign-compare -I ../../tools/portaudio/install/include
EXTRA_LDLIBS =
include ../kaldi.mk

BINFILES =

OBJFILES = online-audio-source.o online-feat-input.o online-decodable.o online-faster-decoder.o onlinebin-util.o

LIBNAME = kaldi-online

ADDLIBS = ../lat/kaldi-lat.a ../decoder/kaldi-decoder.a  ../feat/kaldi-feature.a ../transform/kaldi-transform.a ../gmm/kaldi-gmm.a ../hmm/kaldi-hmm.a ../tree/kaldi-tree.a ../matrix/kaldi-matrix.a  ../util/kaldi-util.a ../base/kaldi-base.a 

include ../makefiles/default_rules.mk


