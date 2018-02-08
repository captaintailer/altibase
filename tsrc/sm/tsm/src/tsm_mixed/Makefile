# $Id: Makefile 68602 2015-01-23 00:13:11Z sbjang $ #

# �����ؾ� ��  ����
#  DESTDIR  : �׽��� ���� ���丮
#  BINS     : ����ȭ�ϸ�
#  BINSRCS  : ����ȭ�� �ҽ�
#  run.sh   : �׽�Ʈ ������ �ҽ� �ڵ�

include ../tsmEnv.mk

PUROPT=-static-checking=yes -static-checking-default=aggressive -static-checking-guardzone=128 -free-queue-length=10000 -free-queue-threshold=100000

DESTDIR = $(TSM_DIR)/tsm_mixed
BINS    = tsm_mixed
BINSRCS = tsm_mixed.cpp testSequence.cpp testSelectiveLoading.cpp\
          testRecovery.cpp testAPI.cpp \
	  testCursor.cpp testDDL.cpp testDML.cpp testRefineDB.cpp \
          testAger.cpp testRebuildIndex.cpp testIsolation.cpp \
          testXA.cpp testLOBInterface.cpp testLOBByTableCursor1.cpp \
	  testLOBByTableCursor2.cpp testLOBFunction.cpp \
          testLOBConcurrency.cpp testLOBRecovery.cpp \
          testLOBStress.cpp testLOBException.cpp

RUN_SHELL= run.sh run1.sh run_lob.sh
BINOBJS = $(BINSRCS:.cpp=.$(OBJEXT))
OBJS=$(SRCS:$(DEV_DIR)/%.cpp=$(TARGET_DIR)/%.$(OBJEXT))
cmd_ld = $(LD) $(LFLAGS) $(LDOUT)$@ $^ $(TSMLIB) $(SMLIB) $(ID_LIB) $(LIBS)

BINSRCS2 = test.cpp
BINOBJS2 = $(BINSRCS2:.cpp=.$(OBJEXT))

all: $(COMPILE) install

compile:  $(OBJS) archive $(BINS)

install: $(BINS)
	$(RM) $(DESTDIR)
	mkdir $(DESTDIR)
	cp $(BINS) $(RUN_SHELL) $(DESTDIR)

tsm_mixed: $(BINOBJS) $(TSMLIB) $(SMLIB) 
	$(cmd_ld)

purify:
	$(RM) tsm_mixed.purify;
	$(MAKE) tsm_mixed.purify

tsm_mixed.purify: $(BINOBJS) $(TSMLIB) $(SMLIB) $(DEPLIBS)
	purify $(PUROPT)  $(LD) $(LFLAGS) -o $@ $<  $(BINOBJS) -ltsm -lsm  $(ID_LIB) $(LIBS)

pure: tsm_mixed.purify install
	cp tsm_mixed.purify $(DESTDIR)/tsm_mixed



test2: $(BINOBJS2) $(TSMLIB) $(SMLIB) $(DEPLIBS)
	$(TSM_MODE) $(LD) $(LFLAGS) -o $@ $(BINOBJS2) -ltsm -lsm $(ID_LIB) $(LIBS)

remake: $(BINOBJS) $(TSMLIB) $(SMLIB) $(DEPLIBS)
	$(RM) tsm_mixed;
	TSM_MODE=purify.old; 
	purify.old $(LD) $(LFLAGS) -o tsm_mixed  $(BINOBJS) -ltsm -lsm $(ID_LIB) $(LIBS)

test:
	clean;
	./tsm_mixed -t 

rerun:
	clean;
	./tsm_mixed

rerun2:
	clean;
	./tsm_mixed -s tsm_selective_loading

rerun3:
	clean;
	./tsm_mixed -s DDL

clean:
	$(RM) core .dependency $(BINS) *.$(OBJEXT) *.d SunWS_cache *~ ./*.*~

dep:
	-$(RM) ./.dependency
	$(CXX) $(CXXOPT_DEPENDANCY) $(INCLUDES) $(DEFINES) $(SRCS) > .dependency

ifeq (./.dependency,$(wildcard ./.dependency))
include ./.dependency
endif




