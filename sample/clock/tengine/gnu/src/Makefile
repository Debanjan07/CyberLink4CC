#
#	Makefile for CyberLink
#
#	Copyright (C) Satoshi Konno 2005
#

DEPS = Dependencies
DEPENDENCIES_OUTPUT := $(DEPS)

# for C++
USE_CPP = 1
REL_TYP=q

include $(BD)/bappl/etc/makerules
include $(CLINKROOT)/lib/tengine/gnu/Makefile.inc

TARGET = upnpclock

S = ../../../ ../src
VPATH = $S

UPNPCLOCKSRC = \
	../src/upnpclock.cpp \
	../../../Clock.cpp \
	../../../ClockDevice.cpp

unexport SRC =
SRC = $(UPNPCLOCKSRC) $(CLINKSRC)

OBJ = $(addsuffix .o, $(basename $(SRC)))

CFLAGS += $(CFLAGS_WARNING)

CFLAGS += -DPROCESS_BASE
ifeq ($(mode), debug)
  LDLIBS += -ltm
endif

CFLAGS += -I$(CLINKHEADERROOT)
CFLAGS += -I$(CPPUNITROOT)/include
CFLAGS += -I$(EXPATROOT)/lib
CFLAGS += -I../../../
CFLAGS += -DTENGINE
CFLAGS += -DTENGINE_NET_KASAGO
#CFLAGS += -DTENGINE_NET_PMC
CFLAGS += -DUSE_XMLPARSER_EXPAT

# for C++
CPPFLAGS += -D__USE_THREADS
CPPFLAGS += -D__USE_IOSTREAMS
LOADLIBES += -lstlport

# for Expat
CFLAGS += -DHAVE_MEMMOVE

# for KASAGO
ifeq ($(MACHINE), m32104)
  START_ADR = -Wl,-Ttext,0x049B0000,-Tdata,0x049B8000
  CFLAGS += -DTM_BIG_ENDIAN
  CFLAGS += -O0
endif
ifeq ($(MACHINE), sh7727)
  CFLAGS += -DTM_LITTLE_ENDIAN
endif
LOADLIBES += -lsvc_ka

# ----------------------------------------------------------------------------

.PHONY: all install clean

ifeq ($(mode), debug)
  NOINST = $(TARGET).trg
endif
INST = $(TARGET) $(TARGET:%=%.map)
ALL = $(INST) $(NOINST)

all: $(ALL)

#$(TARGET): $(OBJ)
$(TARGET): $(addsuffix .o, $(basename $(SRC)))

$(OBJ):

$(TARGET).trg: $(TARGET)
	cp $< $@
	$(STRIP) $@

clean:
	$(RM) $(OBJ) $(ALL) $(DEPS)

prenv:
	set

install: $(addprefix $(EXE_INSTALLDIR)/, $(INST))

$(TARGET:%=$(EXE_INSTALLDIR)/%): $(EXE_INSTALLDIR)/%: %
	$(EXE_INSTALL_STRIP)

ifdef DEPENDENCIES_OUTPUT
  $(DEPS): ; touch $(DEPS)
else
  $(DEPS): $(SRC) ; $(MAKEDEPS) $@ $?
endif
include $(DEPS)
