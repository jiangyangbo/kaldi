
all:

include ../kaldi.mk

TESTFILES = const-integer-set-test stl-utils-test text-utils-test \
    edit-distance-test hash-list-test timer-test kaldi-io-test parse-options-test \
    kaldi-table-test

OBJFILES = text-utils.o kaldi-io.o \
         kaldi-table.o parse-options.o simple-io-funcs.o

LIBNAME = kaldi-util

ADDLIBS = ../matrix/kaldi-matrix.a ../base/kaldi-base.a

include ../makefiles/default_rules.mk


